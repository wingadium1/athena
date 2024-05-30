---
title:  "Optimize Lambda start time"
tags: [AWS, Lambda, Serverless, type/blog]
date: 2019-01-01
---

Gần đây có nghiên cứu lại mấy vấn đề của **[Lambda function](https://aws.amazon.com/lambda/)** và mày mò vào Node
Summit, bài viết này thực ra trình bày
lại [topic này](https://vimeo.com/287511222?fbclid=IwAR31W61fC-MJcXiPdT4ywMKX-ccKiAzviQogcOaivyGpPzcj5vY16A6obCw)
của [Matt Lavin
](https://www.linkedin.com/in/mdlavin/)

Về cơ bản với Lambda, [[AWS]] đã làm gần hết mọi thứ về management, scale function, kết nối đến các service như DynamoDB,
SQS,... Gần như chúng ta chỉ cần chú ý đến việc coding là chính. Tuy nhiên để mọi thứ tốt hơn cho người dùng thì cần
giảm latency, response nhanh hơn và dễ debug hơn trong những trường hợp cần thiết và chính trong source code Lambda
function tức là:

* Cải tiện latency
* Tìm ra bug performance
* Debug.

Bài này sẽ nói về các cách optimze coding là chính, những phần khác thì hãy xem kỹ topic nhé.

Đầu tiên bao giờ cũng cần tìm hiểu xem Lambda hoạt động như nào nhưng trước tiên mình sẽ đưa một ví dụ điển hình về
lambda function:

```nodejs
const dynamodb = require('aws-sdk/clients/dynamodb');
const docClient = new dynamodb.DocumentClient();
const tableName = process.env.SAMPLE_TABLE;
exports.getByIdHandler = async (event) => {
    const { httpMethod, path, pathParameters } = event;
    if (httpMethod !== 'GET') {
        throw new Error(`Unsupported method`);
    }
    console.log('received:', JSON.stringify(event));
    const { id } = pathParameters;
    const params = {
        TableName: tableName,
        Key: { id },
    };
    const { Item } = await docClient.get(params).promise();
    const response = {
        statusCode: 200,
        body: JSON.stringify(Item),
    };
    return response;
};
```

Khá là điển hình với việc: Khởi tạo SDK, handle request, query database và đưa ra kết quả, tất nhiên trước đó sẽ là
download source code và khởi chạy lambda function. Và hãy ghép nó vào mô hình lifecycle của lambda function như ở bên
dưới.
