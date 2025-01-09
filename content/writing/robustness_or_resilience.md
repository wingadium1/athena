---
title: "Robustness hay Resilience?"
tags: [ continuous_delivery, developer, devops, writing, robustness, resilience, type/write ]
---

Nếu phần mềm là một vật thể bạn mong muốn chúng như một chai thủy tinh rất cứng hay một sợi dây cao su có khả năng co
giãn?

Đầu tiên nói đề cập thế này có vẻ hơi khó hiểu, hãy quay trở lại với bài
viết: [[writing/Software-Architecture-Origin/software-arch-bat-dau-tu-dau-3|Software Architecture: Bắt đầu từ đâu? – Part 3 Soft Skills – Continuous Delivery]].
Chúng ta có một vài metric liên quan đến Continuous Delivery, hãy để ý đến 2 cái Mean Time Between Failures (MTBF) và
Mean Time To Repair ([[refs/mean_time_to_recovery|MTTR]]).

Robustness, một quan niệm khá xưa trong thế giới phần mềm, người ta muốn quan tâm đến việc phần mềm ít xảy ra sự cố hơn
là thời gian khắc phục nó (ngay cả ở môi trường thương mại - production), tức là MTBF dài hơn được coi trọng hơn [[refs/mean_time_to_recovery|MTTR]]
ngắn. Cách tiếp cận này đôi khi được coi là truyền thống, và thường các phần mềm này sẽ không được apply Continuous
Delivery trong quy trình phát triển.

Resilience, khả năng phục hồi, thì ngược lại [[refs/mean_time_to_recovery|MTTR]] sẽ được coi trọng hơn, một cách tiếp cận để hạn chế tác động của sự cố
phần mềm và phần mềm có thể hoạt động trong các điều kiện khác nhau về phần cứng cũng như cơ sở hạ tầng.

Một cách suy nghĩ đơn giản như này, với Robustness phần mềm khó có cơ hội apply [[refs/CD|Continuous Delivery]], vì khi một phần mềm
đòi hỏi ít sự cố, tức là số người trong chuỗi ra quyết định sẽ dài hơn (quan liêu), các stage cuối cùng của CD Pipeline
thường ít được sử dụng do cần thời gian dài để xử lý qua các stage trước đó.

Khi một phần mềm có xu hướng chuyển dần sang Resilience thì chúng ta có thể giảm effort để quản lý rủi ro (vì sự cố và
lỗi được xử lý liên tục), và dần chuyển sang CD.

## Robustness - Truyền thống

Dễ thấy là trong 1 2 chục năm trước, phần mềm hoàn hảo luôn là xu thế, các tổ chức luôn muốn phần mềm có độ tin cậy cao,
tức là MTBF/[[refs/mean_time_to_recovery|MTTR]] khá lớn.
Các dự án phần mềm theo kiểu này luôn muốn duy trì một môi trường production không có sự cố, với một niềm tin rằng môi
trường production luôn được xử lý theo các ẩn số đã biết, trong đó các quá trình tương tác với môi trường luôn đồng nhất
và có thể dự đoán được. Sự cố trong các phần mềm này luôn được cho là do các thay đổi (source code, biến môi trường, các
thông số phần cứng), và có thể dự đoán được.
> Trong một vài dự án với các khách hàng mang xu thế cổ điển, họ thường đòi hỏi chúng tôi chỉ ra tất cả các kịch bản có
> thể có với hệ thống phần mềm mới được xây dựng????

Trong trường hợp đó, chúng ta chỉ có thể xử lý theo cách như sau:

![image](https://cdn-media-1.freecodecamp.org/images/2hCjNrylujhop6fe8MXH7vzINh0ek6eqCRqh)

* End-to-end testing để xác minh chức năng trong phiên bản mới mới cùng với các service/component phụ thuộc.
* Change Boards: một bộ sậu các phòng ban để quyết định đưa phiên bản lên môi trường production
* Freeze: đóng băn phần mềm để hạn chế thay đổi trong một khoảng thời gian.

Đầu tiên chúng ta có thể nghĩ rằng, tại sao việc chú trọng vào phần mềm không lỗi lại là vấn đề trong khi nó là một yêu
cầu khá hợp lý, nhưng hãy xem xét, 3 practice trên đều chậm, đó là vấn đề của
việc [quản lý rủi ro trong Continuous Delivery](https://continuousdelivery.com/2013/08/risk-management-theatre/).
End-to-end testing đòi hỏi chi phí và thời gian dài để vét cạn các lỗi, đồng thời khi đến giai đoạn System test thường
sẽ cần thời gian down time đáng kể để cô lập các vấn đề ở môi trường production. Change Boards sẽ dẫn đến một cơ chế
quan liêu để đưa ra được quyết định cho phiên bản mới. cả 2 practice đều chậm và vẫn có khả năng sự cố, tức là chúng ta
không thể thấy được các ẩn số chưa biết trong hệ thống phức
tạp ([Complex domain - Cynefin framework](https://en.wikipedia.org/wiki/Cynefin_framework#Complex),
còn chúng ta mới chỉ để ý đến Complex Domain)

![image](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/Cynefin_framework_by_Edwin_Stoop.jpg/2560px-Cynefin_framework_by_Edwin_Stoop.jpg)

Freeze, đương nhiên rồi, lead time quá dài.

Thời gian trong việc phát triển phần mềm hiện nay luôn là yếu tố quan trọng, chậm trễ vài tháng thậm chí chỉ là vài tuần
sẽ làm lỡ các phiên bản update của library, phần cứng hay ngôn ngữ, việc đó sẽ gia tăng rủi ro khi chúng biến thành các
yêu cầu thay đổi về công nghệ, phiên bản vì có quá nhiều việc phải xử lý trong một lần update phần mềm.

Từ đó có thể thấy rằng, Robustness hoàn toàn không thể đáp ứng nhu cầu về mặt kinh doanh của phần mềm, vì sự hạn chế về
sự ổn định và số lượng phiên bản, điều đó lý giải tại sao nhiêu dự án phần mềm không thể xử lý được vấn đề [[refs/CD|Continuous
Delivery]].

## Khi sự cố là điều không thể tránh khỏi

Vấn đề là thường khi chúng ta quá chú ý đến Robustness, phần mềm thường quá chặt chẽ và dẫn tới việc phần mềm đó gần như
không có khả năng xử lý khi có failure, tức là cả hệ thống sụp đổ, thay vì một phần nhỏ.

Chúng ta có thể thấy như này khi một dự án chú ý đến Robustness, nhưng họ lại bị thúc ép bởi các nhu cầu mang tính ngắn
hạn (nhanh, rẻ, tốt) thì các nhu cầu cơ bản về an toàn, high availability sẽ bị bỏ qua. Dự án sẽ tập trung rất nhiều
nguồn lực vào môi trường production, các non-functional requirement thường sẽ bị bỏ qua, không có các dự tính dài hạn
cho môi trường thực tế khi user bắt đầu sử dụng, hạ tầng thường dễ bị sụp đổ (fragile) bởi các yếu tố bất ngờ về người
dùng (số lượng người dùng gia tăng đột biến). Nhưng ngược lại họ và đối tác thường chấp nhận vì hệ thống hiếm khi xảy ra
sự cố.

Tuy nhiên, thật là production environment không hẳn là một hệ thống phức tạp mà chúng ta có thể dự đoán được. Production
environment theo kinh nghiệm hiện tại cho thấy, thường sẽ phải xử lý khối lượng lớn các request không đồng nhất, với các
điều kiện không thể lặp lại.
> Ví dụ phần mềm sẽ xử lý thế nào nếu AWS bị sự cố ở cả 1 datacenter (Availability Zone)
> Nhìn chung các hệ thống này thường sẽ tiềm ẩn nhiều sai sót nhỏ lẻ, nhưng không tạo thành sự cố. Mà thường các sự cố
> này
> được tạo ra trong một điều kiện nhiều sai sót nhỏ lẻ kết hợp với nhau.

Vậy khi có sự cố chúng ta mất những gì

* Đầu tiên là `doanh thu = doanh thu trên một đơn vị thời gian x thời gian để xử lý`
* Chi phí mất mát vì sự cố đến khi chúng được phát hiện: tỉ như việc nhà mạng không thể xử lý kịp khi tài khoản hết tiền
  mà vẫn nhắn được hàng trăm, hàng chục tin trong vòng vài phút, sự cố huyền thoại này có lẽ mất đến hàng năm để phát
  hiện. Rõ ràng chi phí cho sự cố tiềm ẩn này quá lớn.
* Chi phí về cơ hội tiếp cận khách hàng, giả sử như Tiki, họ không thể xử lý được các hot deals trong các đợt sale trong
  thời gian thực ([ref](https://engineering.tiki.vn/how-to-handle-hot-deals-at-the-peak-time-821908b9d340)) thì họ sẽ
  mất đi khá nhiều khách hàng vì niềm tin, hơn nữa với kiến trúc dễ
  vỡ, [disaster blast radius](https://www.oreilly.com/library/view/chaos-engineering/9781491988459/ch07.html) có thể
  lớn, ảnh hưởng đến các vùng khác trong hệ thống.

Hãy xem xét bài toán giả thuyết như này:
> Chúng ta có một trang thương mại điện tử, vì một lý do nào đó, database của service Cart bị quá tải, mọi thứ vẫn hoạt
> động, nhưng người dùng không thể đặt hàng. Đội dự án không phát hiện ra sự cố này cho đến ngày thứ 4, mất 2 ngày để
> hot fix, và mất 1 ngày tiếp theo để checkout. Mỗi ngày chúng ta mất 80 triệu doanh thu, vậy là tổng cộng chúng ta mầt 560
> triệu, trong đó có 240 triệu chi phí cơ hội và 320 triệu chi ví chìm.

![image](https://s3.ap-southeast-1.amazonaws.com/techover.storage/wp-content/uploads/2021/08/24171154/cost.png)

Tại sao sự cố lại xảy ra trong khi tất cả mọi việc đã được tính toán trước? Con người, tất nhiên rồi. Có một lý thuyết
có tên gọi là [Bad Apple - Quả táo hỏng](https://en.wikipedia.org/wiki/Bad_apples), trong trường hợp này có thể hiểu là, không phải tất cả các thành viên
trong dự án đều hoàn hảo mọi lúc (về công việc của họ trong dự án), hệ thống tin cậy nhưng con người thì không, khi có
sự cố xảy ra nó kết hợp với việc tất cả mọi thứ đều được xem như là đã tính toán trước (các ẩn số đã biết), dễ dàng trở
thành một kiểu đổ lỗi cho các cá nhân có liên quan, từ đó giảm độ hợp tác và chia sẻ kiến thức chung trong dự án.

> Việc tách biệt các nhiệm vụ đóng vai trò như một rào cản đối với việc chia sẻ kiến thức, phản hồi và cộng tác, đồng
> thời làm giảm nhận thức về tình huống vốn là yếu tố cần thiết để có phản ứng hiệu quả trong trường hợp xảy ra sự cố.
> Hơn nữa, thường trong develop team sẽ còn có các hoạt động review code, điều này cũng làm giảm hiệu suất cho việc xử
> lý/phản hồi về sự cố, và đổ lỗi cho hoạt động review.

Với đội dự án trong trường hợp thương mại điện tử ở trên, họ sẽ xử lý như nào, có lẽ để xây dựng một phần mềm theo hướng
Robustness, họ đã mất cả tháng (Lead Time), để qua tất cả các practice. Thế nhưng họ đã mất 4 ngày doanh thu cho chi phí
chìm vì phát hiện chậm, vậy nên 320 triệu đã mất đi khiến họ phải chạy đua với thời gian để giảm ngay lập tức chi phí cơ
hội, ở trường hợp này họ sẵn sàng hy sinh chính Robustness để đảm bảo tiến độ, trong đó tất cả các practice của
Robustness bị giảm xuống còn giờ hoặc ngày. Vấn đề có thể thấy ngay, cùng một quy trình nhưng đã bị kéo từ hàng tuần
xuống hàng ngày hoặc thậm chí vài giờ.

[[refs/CD|Continuous Delivery]] có thể cải thiện độ ổn định của hệ thống và khối lượng release, nhưng để xử lý rất khó.
[[refs/CD|Continuous Delivery]] cho hệ thống Robustness sẽ gặp vấn đề ngay lập tức khi dự án quá chú trọng vào khối lượng
release (đơn giản là khối lượng lớn, nhưng thời gian release quá dài để thông qua các practice). Rõ ràng thời gian development
(coding, design, architect, unit-test) khó có thể giảm, vì vậy 3 practice rõ ràng là khả dĩ nhất để thay đổi và giản lược, nhưng
End-to-end testing gần như đã ăn sâu vào tiềm thức và gần như mọi người đều công nhận nó. Và [[refs/CD|Continuous Delivery]]
sẽ bị đổ lỗi vì làm thay đổi văn hóa của dự án ngay lập tức, khi có lỗi đầu tiên ở môi trường production sau khi áp dụng, và dự
án sẽ quay trở lại với hiện trạng.

## Nếu hệ thống có khả năng phục hồi thì sao

Hoặc chí ít là phục hồi nhanh, nếu việc này được chú trọng (Resilience), tức là chúng ta muốn có [[refs/mean_time_to_recovery|MTTR]] thấp hơn là việc
MTBF cao, bằng cách tối ưu hệ thống để có thể hotfix trên production khi có sự cố. Nhìn chung lỗi có thể phân loại, một
số sẽ không bao giờ xảy ra, một số lỗi gây ra sự cố nghiêm trọng hơn các lỗi khác và các lỗi về an toàn thì không được
phép xuất hiện, nhưng rõ ràng chúng ta nên có thể nhanh chóng đưa hệ thống trở lại hơn là cố gắng ít sự cố hơn.

Nếu giả sử trang thương mại điện tử bên trên có khả năng phục hồi (tự động hoặc manual), chúng ta có thể dễ dàng thay
đổi hệ thống về phiên bản ổn định trước đó, ngoài ra khi hệ thống gặp vấn đề về performance, việc thay đổi phần cứng tốt
hơn sẽ dễ dàng hơn và chi phí phần cứng đôi khi chưa bao giờ là vấn đề khi so sánh với mất mát về doanh thu.

![image](https://books.openedition.org/pressesmines/docannexe/image/1115/img-1-small700.jpg)

Vậy xây dựng hệ thống Resilience cần gì, theo Erik Hollnagel trong
cuốn [Resilience Engineering in Practice](https://erikhollnagel.com/books/resilience-engineering-in-practice.html) có 4
yếu tố:

* Anticipation: Hiểu hệ thống sẽ đối mặt với những thứ gì: số lượng người dùng đột biến khi có notification mới, một API
  Get cho hàng triệu người dùng mà không có biện pháp cache sẽ gây quá tải database và web server...
* Monitoring: biết được cần theo dõi thứ gì trong hệ thống, khi nào hệ thống bất thường
* Response: sử dụng các hiểu biết và các nguyên tắc cũng như nhận thức chung của cả dự án khi có sự cố để giảm thiểu tác
  động.
* Learn: Hiểu các trường hợp sự cố là gì, các trường hợp có cảnh báo, và chia sẻ trong dự án.

Ngoài ra sẽ có các yếu tố về văn hóa và tổ chức hỗ trợ. Trong một dự án, rõ ràng với hệ thống phức tạp, dự án cần một
khối lượng kiến thức và nguồn lực đủ nhiều để ngăn ngừa các lỗi tiềm ẩn hoặc để xử lý sự cố, thường thì các team scrum
sẽ có các phương thức hotfix để xử lý sự cố nhỏ hơn là thực hiện theo các operation guideline, và tất cả đều dựa trên sự
giao tiếp thuận tiện giữa tất cả các thành viên, clear and transparent information.

Vậy Resilience system cần gì, chúng ta sẽ có một môi trường production mà trong đó các service/component độc lập, mở
rộng theo chiều ngang và dọc tốt để ứng phó các thay đổi bất ngờ trong thực tế. Nhìn chung ngưỡng chịu đựng của các hệ
thống Resilience sẽ tốt hơn: ví dụ nếu hệ thống phân tán và phần cart có khả năng mở rộng thì có thể hệ thống thương mại
điện tử bên trên có thể serve được đến 10.000 CCU mới xảy ra sự cố thay vì 3.000 CCU, như vậy thời gian down time và chi
phí mất đi rõ ràng giảm.

Vậy thay vì quá chú trọng vào phần DEV, phần OPS trong dự án được đầu tư nguồn lực tốt hơn để giữ cho hệ thống ở trong
điều kiện hoạt động an toàn và tin cậy, tạo nên một cán cân cân bằng hơn trong [[refs/DevOps]]. Tuy nhiên việc xây dựng hệ thống có khả năng chịu đựng tốt với các điều kiện,
thường dựa trên các practice sau đây:

1. Đối với phần developer:

* Adaptive architecture hay hiểu theo kiểu defensive architecture cũng được: code những gì cần thiết, tính toán các
  trường hợp, có một bức tranh tổng thể, code dễ hiểu, dễ maintain và có thể mở rộng
* Feature On/Off, feature không gắn liền với phần kết cấu lõi của phần mềm, ví dụ trong một hệ thống IoT, timeseries có
  thể lưu trữ bằng nhiều loại DB khác nhau: Timeseries Database ~ [Cassandra](cassandra), hoặc đơn giản hơn lưu trữ vào
  thẳng một DB
  quan hệ với index phù hợp, khi [Cassandra](cassandra) down, rõ ràng việc dựng lại cả hệ thống DB Cassandra mất nhiều
  thời gian hơn
  cho việc chuyển sang SQL, nhưng việc đó cần được chuẩn bị về mặt feature

2. Test, mình muốn nói đến việc test tình trạng hệ thống nhé, còn feature đương nhiên phải pass all test rồi.

* Smoke test kết hợp với [Chaos Engineering](https://principlesofchaos.org/) sẽ hiệu quả trong việc kiểm thử về tình
  trạng sơ bộ của hệ thống hoặc tìm ra các lỗi tiềm ẩn như trong các điều kiện server bất ngờ dừng hoạt động hay ổ cứng
  bị lỗi, kết nối mạng bị ngắt...

3. Hạ tầng - Infrastructure: Infrastructure as a service và Auto Recovery, IaaS sẽ đảm bảo việc hạ tầng ở các môi trường
   giống nhau, giảm tác động của con người với hạ tầng, trong khi đó Auto Recovery sẽ phục hồi hệ thống khi có service
   bị fail, tất nhiên ở mức độ nào đó, source code của developer sẽ cần đáp ứng được một số yêu cầu.

> Những ai đã tiếp xúc với các kỹ thuật [IaaS](iaas) như terraform, pulimi, cloudformation, k8s yaml đều khá tự tin khi
> deploy
> một môi trường tương tự, độ chính xác của các phần code hạ tầng này đều rất cao, công việc còn lại chỉ là đánh giá lại
> kết quả.

4. Timeseries/Telemetry: Log, Monitor luôn là yêu cầu bắt buộc, kết hợp với alarm để mọi người có thể được thông báo khi
   có các sự cố hay lỗi runtime xảy ra. Cuối cùng là User Analytics (có thể lấy từ Firebase analytic, ...) dành cho việc
   theo dõi user experience.
5. Con người: phần luôn là khó nhất và để nói về văn hóa trong dự án là chính, nhưng có nguyên tắc ["You Built It, You Run It"](https://aws.amazon.com/blogs/enterprise-strategy/enterprise-devops-why-you-should-run-what-you-build/):
   thông tin trong dự án sẽ cần được transparency, mọi người cần làm mọi việc có thể làm (có thể không thuộc trách nhiệm
   của mình) để đảm bảo rằng mọi thứ ổn ngay từ đầu, việc minh bạch thông tin sẽ giúp tìm ra nguyên nhân của các vấn đề
   nhanh hơn nhiêu.

1 trong 2 yếu tố nữa trong phần con người đó
là [Blameless PostMortems](https://codeascraft.com/2012/05/22/blameless-postmortems/), mọi người rất dễ mắc sai lầm nhất
là khi họ cần cân bằng giữa khả năng và trách nhiệm, khi đó không nên điều tra nguyên nhân và quy kết cho sự cố xuất
phát từ sai lầm cá nhân, điều chỉ nên được xem xét như là một nguy cơ tiềm ẩn trong tổ chức, vấn đề này gần như luôn
luôn song hành với yếu tố còn lại - [Hindsight bias](https://en.wikipedia.org/wiki/Hindsight_bias). Thành kiến ​​nhận
thức muộn - Khi mà mọi người vẫn tin rằng sau một sự cố trong quá khứ, họ có thể dự đoán được với độ chắc chắn cao về
các sự cố có thể xảy ra sau này, điều đó có nghĩa là khi bạn để xảy ra lại những sự cố đó thì đó là thảm họa. Nhìn chung
nên nhìn nhận những vấn đề kiểu như Hindsight Bias là không thể tránh khỏi và hãy làm tất cả để tránh cùng với việc hãy
để những người mắc sai lầm truyền đạt lại kinh nghiệm của họ để tránh sự cố cho những người khác.

Hơi lan man một chút, hãy quay trở lại bài toán thương mại điện tử nêu trên. Trong trường hợp build hệ thống đó theo
hướng Adaptive và Feature On/Off, rõ ràng phần cart luôn là phần quan trọng trong hệ thống, nó nên được quản lý bởi
một [Service Registry](https://microservices.io/patterns/service-registry.html) bên cạnh
đó [Circuit Breaker](https://microservices.io/patterns/reliability/circuit-breaker.html) sẽ đóng vài trò kiểm tra
service có được hoạt động hay không, vậy sẽ không tốn đến 5 phút để phát hiện ra vấn đề, một bản vá có thể được fix
trong vòng 3 tiếng, thâm chí là 1 ngày nhưng sẽ được deploy ngay sau đó. Hãy tính toán lại chi phí: như vậy sẽ tốn ~ 80
triệu, trong đó chi phí chìm vì phát hiện ra vấn đề chậm không đáng kể, còn chi phí fix cũng giảm do việc deploy nhanh
chóng và thuận tiện hơn.

Khả năng phục hồi được tối ưu hóa gần như là nhu cầu tất yếu cho một phần mềm hay một tổ chức có thể đổi mới và phát
triển, khi đó chúng ta có thể đầu tư vào con người và công nghệ để đạt được Robustness: khả năng thích ứng với các điều
kiện bất ngờ có thể xảy đến trong tuơng lai, khi đó các doanh nghiệp, tổ chức sẽ có lợi thế lớn khi đưa ra sản phẩm
nhanh hơn các đối thủ cạnh tranh.

## Continuous Delivery hỗ trợ bởi Resilience

Mọi người sẽ có xu hướng tìm một cách để xử lý Continuous Delivery cho mọi dự án, tổ chức tuy nhiên việc đó quá phức
tạp. Bản thân việc xây dựng theo hướng adaptive architect như đã nói ở trên cũng sinh ra các hoàn cảnh và ràng buộc.

Tuy nhiên nếu một dự án đã tối ưu hóa Robustness và đang không có [[refs/CD|Continuous Delivery]] chúng ta có thể xử lý theo các
bước sau.

1. Version hóa mọi thứ: Deployment sẽ ổn định hơn, không có phiên bản nào bất ngờ được deploy mà mọi người không biết.

> Có một vài tình huống như bạn thực hiện hàng loạt thay đổi trong K8S Yaml, nhưng ngày hôm sau sự cố xảy ra, bạn không
> thể biết được chính xác vấn đề nằm ở đâu khi không versioning các config mà bạn đã làm.

2. Đo đạc: mục đích đảm bảo việc delivery ổn định hơn khi chúng ta đã biết rõ chúng ta phát triển thứ gì và
   delpoy/deliery những phần nào.
3. tăng độ tin cậy cho Production environment: có 2 hoạt động cần làm rearchitect, và thiết lập các hệ thống monitoring.
   Rearchitect cần làm dần dần và theo hướng adaptive để giảm rủi ro do vẫn là hệ thống production.
4. Bước cuối cùng, thực ra đây không hẳn là một bước mà khi đến bước này hệ thống và dự án đã đủ các điều kiện cần
   thiết, việc deployment đã ổn định production environment đã phần nào có khả năng tự phục hồi, việc còn lại là áp dụng
   như một hệ thống Resilience.

## Sau đó thì sao

Nhìn chung sau các bước bên trên, điều còn lại cuối cùng là văn hóa. Các tổ chức có xu hướng vẫn giữ lại các practice
của robustness để đề phòng rủi ro, việc thay thế nó cần thời gian và một câu chuyện hợp lý bao gồm các lập luận về chi
phí, thời gian và tỉ lệ lỗi giảm.

Nhưng việc đầu tiên đó là thay đổi nhận thức về các vấn đề quản lý rủi ro và robustness. Các bước thay đổi bên trên sẽ
cho chúng ta một loạt các thông số về độ ổn định, khối lượng delivery và minh họa chi phí có thể được cải tiến cũng như
[[refs/mean_time_to_recovery|MTTR]] và thời gian triển khai. Ngoài ra Chaos Engineering được xem xét cẩn thận sẽ cho chúng ta các practice giúp giảm
[[refs/mean_time_to_recovery|MTTR]] xuống vài giờ thậm chí vài phút, bằng cách giả lập và chỉ ra các lỗi có thể xảy ra trong môi trường production.

Các practice của robustness khi đó sẽ dần được thay thế bằng sự kết hợp giữa [[refs/CD|Continuous Delivery]] và các practice trong
quá trình vận hành. End-to-end testing sẽ được thay thế dần
bởi [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html), khối lượng test, tần suất test sẽ được phân phối
một cách hợp lý để giảm thời gian test và chi phí. Trong đó số UnitTest sẽ được tiến hành thường xuyên và cho môi bản
build, Acceptance Test sẽ được pick up (khoảng 100 cases) và cũng được tiến hành như vậy, trong khí đó các loại smoke
test sẽ chỉ khoảng 10 case, các kỹ thuật telemetry thì gần như tiến hành hằng ngày hằng giờ.

Change Boards và Change Freezes hiện tại sẽ trở nên lỗi thời với các kỹ thuật Blue Green Deployments và Canary
Deployment, trong đó các phiên bản mới sẽ được triển khai và thay thế dần dần phiên bản cũ, việc deploy sẽ được rollback
khi có sự cố. Ngoài ra Facebook cũng đưa ra một kỹ thuật
khác - [Dark Launch](https://tech.co/news/the-dark-launch-how-googlefacebook-release-new-features-2016-04), khi đó
Facebook sẽ đưa các tùy chọn cho người dùng có bật hay tắt các tính năng mới hay không. Hiện nay Google hay AWS cũng làm
theo cách này, cũng là một cách khá tốt để thử nghiệm tính năng trên môi trường thực tế - điều mà mọi dự án đều mong
muốn.

## Kết luận

Tối ưu hóa cho Robustness có vẻ đã lỗi thời và không thể đáp ứng được nhu cầu công nghệ thông tin hiện nay, cùng với đó
việc không áp dụng [[refs/CD|Continuous Delivery]] trong một thời gian dài rất dễ dẫn đến những sự cố lớn. Robustness rõ ràng vẫn có
giá trị, điều đó là mong muốn tự nhiên của các lập trình viên và dự án, tuy vậy sự ưu tiên nên được thay đổi để hệ thống
có khả năng thích ứng cao hơn cả về nghiệp vụ và các yêu cầu từ phía người dùng.

Mặt khác tối ưu hóa cho khả năng phục hồi của hệ thống có vẻ là một chiến lược có độ tin cậy cao hơn, cho phép phần mềm
và dự án có thể mở rộng, tránh tác động của các lỗi và tìm cách làm cho phần mềm có khả năng chịu đựng và thích ứng tốt
hơn. Việc này rõ ràng là một sự thay đổi về mô hình tổ chức và văn hóa, trong đó cần mọi người nhận thức rằng hệ thống
phần mềm rõ ràng luôn phức tạp và nhiều khi sự cố là không thể tránh khỏi, việc tối ưu khả năng phục hồi sẽ làm giảm chi
phí hay tổn thất cho sự cố mà thôi.

Ngoài ra việc tối ưu hóa cho Robustness sẽ làm giảm hiệu quả của [[refs/CD|Continuous Delivery]], [[refs/CD|Continuous Delivery]]
có thể cho ta thấy hiệu quả rõ ràng với các hệ thông phục hồi tốt, điều đó giúp chúng ta tăng độ tin cậy từ source code, deployment,
delivery và môi trường production ngay từ đầu.
