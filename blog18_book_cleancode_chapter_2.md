# Book - Clean Code - Meaning Name

# Giới thiệu

Có một câu chắc hẳn mọi người đã được nghe ở đâu đó:
_"There are only two hard things in Computer Science: cache invalidation and naming things."_ – Phil Karlton.
Đại ý là có 2 thứ khó trong khoa học máy tính đó là giải quyết cache và đặt tên. Mở rộng ra thì đặt tên là vấn đề ở khắp mọi nơi. Tên ở mọi nơi xung quanh chúng ta, và đặc biệt là trong lập trình.
Chúng ta đặt tên cho biến, hàm, class, file, thư mục. Để đặt tên tốt yêu cầu có nhiều kinh nghiệm và không dễ. Ở chương 2 của Clean Code, tác giả viết về Meaning Name. Chúng ta cùng tìm hiểu xem những bí quyết xương máu của tác giả trong việc đặt tên.

# Sử dụng tên thể hiện được mục đích

- Điều này khi nghe có vẻ là dễ, nhưng thực ra không dễ chút nào, nó khiến lập trình viên tiêu tốn rất nhiều thời gian.
- Tên phải tự trả lời cho các câu hỏi về nó, như nó sinh ra để làm gì, nó có kiểu dữ liệu gì, nếu cần phải comment tức là nó chưa tốt.

```
int d; //elapsed time in days
```

Biến d không thể hiện được gì, khi có comment thì người đọc sẽ hiểu, nhưng nó thực sự không tốt.
Thay vào đó chúng ta nên sử dụng:

```
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

Nhìn vào tên của biến, ta có thể mường tượng được ý nghĩa của biến chứ không cần comment.
Sử dụng tên thể hiện được mục đích sẽ khiến code dễ hiểu hơn và dễ cho người khác bảo trì.
Vd cho đoạn code sau:

```
public List<int[]> getThem() {
List<int[]> list1 = new ArrayList<int[]>();
for (int[] x : theList)
if (x[0] == 4)
list1.add(x);
return list1;
}
```

Đọc đoạn code trên, rất khó để hiểu đoạn code trên làm gì, những biến trên có tác dụng gì. getThem, list1, theList? chúng là gì?
theList chứa cái gì?
x[0] tại sao lại bằng 4?
list1 là gì?
Dựa vào đoạn code trên ta rất khó mường tượng được các câu hỏi trên.
Nếu trong ngữ cảnh đang làm game dò mìn. Ta có thể biến đổi:
theList => gameBoard
x[0] là trạng thái đầu tiên của mỗi ô => cell[STATUS_VALUE]
4 là trạng thái gán cờ => FLAGGED

```
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell : gameBoard)
      if (cell[STATUS_VALUE] == FLAGGED)
        flaggedCells.add(cell);
    return flaggedCells;
}
```

Ta có thể cải tiến đoạn code trên bằng cách viết 1 lớp cho các ô (cell) thay vì dùng kiểu mảng int, trong đó có thể biểu diễn 1 hàm để kiểm tra xem có phải flag hay không, kĩ thuật này gọi là `magic number`

```
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<Cell>();
    for (Cell cell : gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
    return flaggedCells;
}
```

## Tránh sai lệch thông tin

- Chúng ta cần đặt tên theo đúng nghĩa của nó
- Không nên đặt một nhóm tài khoản là accountList nếu nó không phải là 1 danh sách (List), có thể sử dụng accountGroup hoặc bunchOfAccounts hoặc đơn giản chỉ là accounts
- Cẩn thận với những tên giống nhau, `XYZControllerForEfficientHandlingOfStrings` và `XYZControllerForEfficientStorageOfStrings` => thật tồi tệ
- Tránh sử dụng l, O vì chúng rất dễ nhầm lẫn, l nhầm sang 1 và O nhầm sang 0

## Sử dụng nghĩa khác nhau

Chúng ta không nên đổi tên tuỳ tiện khi code để chỉ đạt mục đích là cho chương trình chạy được. Một ví dụ điển hình là việc không thể tạo tên `class` nên nhiều người để đổi tên thành `kclass`, hoặc khi muốn thể hiện giá trị lớn nhất nhiều ngôn ngữ không cho đặt tên biến là `max` nhiều người đã đổi thành `max_`. Điều này thực sự không tốt.

Không sử dụng những từ gây nhiễu như Info hay Data, Vd trong chương trình có 2 biến là productInfo và productData, thật khó để phân biệt 2 biến này.

Tránh những từ không cần thiết, `variable` không xuất hiện trong tên biến và `table` không xuất hiệu trong tên bảng. `nameString` sẽ không mang nhiều ý nghĩa hơn `name` bởi vì `name` đã là string rồi. Còn nếu `name` là 1 số thì lại vi phạm nguyên tắc **tránh sai lệch thông tin**.

Một vd minh hoạ, giả sử trong mã nguồn có những thứ như sau:

```
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

Ta chẳng biết phải dùng hàm nào cả, nếu không đọc kĩ phần implement của hàm

## Dùng những tên có thể phát âm

Đây là 1 nguyên tắc rất chuẩn, nếu chúng ta làm việc trong cùng 1 team, hoặc khi báo cáo, thật khó nếu ta sử dụng những cái tên khó phát âm.
Vd: `genymdhms` => generation date, year, month, day, hour, minute, second
Thật là khó để giao tiếp với tên biến này, thay vào đó ta sử dụng => `genTimestamp` hoặc `generationTimestamp` sẽ dễ dàng hơn

## Dùng tên tìm kiếm được

Hạn chế sử dụng tên có 1 chữ cái, nếu có thể biến hằng số thành 1 biến thì hãy làm. Vì nếu chúng ta sử dụng biến 1 chữ cái hoặc số thì rất khó để tìm kiếm trong hàng ngàn dòng code.

Người ta có thể tìm được `MAX_CLASSES_PER_STUDENT` dễ dàng nhưng nếu chỉ là số 7 thì lại là công chuyện khác.

ý kiến của tác giả: **The length of a name should correspond to the size of its scope** đặt những tên biến ngắn cho những biến cục bộ chỉ sử dụng trong phương thức nhỏ.

```
for (int j=0; j<34; j++) {
  s += (t[j]*4)/5;
}
```

thành

```
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j < NUMBER_OF_TASKS; j++) {
  int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
  int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
  sum += realTaskWeeks;
}
```

## Tránh mã hoá

Chúng ta làm việc theo nhóm và không phải ai trong nhóm cũng thích dùng những bộ ngôn ngữ khác biệt. Do đó hãy chỉ sử dụng những tên mang tính phổ biến và đại trà, đừng cố mã hoá nó theo phong cách của bạn.

Chúng ta cũng không cần phải thêm các tiền tố cho tên, không cần sử dụng m\_ vào biến thành viên. Mọi người sẽ nhanh chóng bỏ qua tiền tố (hoặc hậu tố) mà chỉ xem phần ý nghĩa của tên.

## Tránh khủng bố người khác

Những người đọc code của bạn sẽ phải điên đầu nếu bạn đặt tên không chính xác.
Một vòng lặp có thể sử dụng i, j, k nếu phạm vi của nó là nhỏ và không xung đột. Điều này xảy ra do chúng ta đã quá quen thuộc với việc này, nhưng trong hầu hết các trường hợp, tên 1 chữ không là lựa chọn tốt.

Tốt nhất đừng hack não người khác, lập trình viên khá thông minh và muốn thể hiện mình, nhưng hãy là những người lập trình viên chuyên nghiệp - người chuyên nghiệp hiểu rằng sự rõ ràng được đặt lên hàng đầu.

## Tên lớp

Tên lớp và các đối tượng nên sử dụng danh từ hoặc cụm danh từ như Customer, Account ... Tránhh những từ gây khó hiểu như Manager, Processor, Data hay in Info. Tên lớp đừng đặt là động từ

## Tên hàm

Tên hàm nên là động từ hoặc cụm động từ.
có thể dùng get, set hay is làm tiền tố trong 1 hàm của class theo chuẩn javabean.

Khi các hàm khơi tạo nạp chồng (overload), sử dụng các phương thức tĩnh(static) có tên giải thích sẽ tốt hơn. Vd:

```
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

sẽ tốt hơn hẳn

```
Complex fulcrumPoint = new Complex(23.0);
```

## Đừng cố trở nên dễ thương trong code

Tên phải rõ ràng, nếu bạn muốn thể hiện sự cute khi đặt tên, nó có thể gây khó chịu cho người khác đọc.
_Say what you mean. Mean what you say._
(Nói những gì bạn nghĩ. Nghĩ những gì bạn nói)

## Chọn 1 từ cho 1 nghĩa

Trong hàng nghìn dòng code, hãy cố thống nhất một nhiệm vụ cho 1 từ. Đừng sử dụng cả fetch, retrieve và get trong 1 source code.

Tương tự rất khó hiểu khi controller, manager và driver lại cùng có trong 1 mã nguồn.
Dùng 1 từ đồng nhất cho toàn bộ ngữ nghĩa là 1 ân huệ của bạn dành cho những người đọc code của bạn. Nam mô a di đà !!

## Đừng chơi chữ

Tránh dùng cùng 1 từ cho 2 mục đích, sử dụng 1 thuật ngữ cho 2 ý tưởng khác nhau về cơ bản là 1 cách chơi chữ.

## Dùng thuật ngữ

Hãy nhớ rằng những người đọc code của bạn là những lập trình viên, do đó hãy sử dụng các thuật ngữ khoa học, các tên mẫu cho việc đặt tên.

## Thêm ngữ cảnh

Có 1 số tên sẽ thể hiện được ý nghĩa của chúng, nhưng không phải tất cả. Do đó, ta nên thêm ngữ cảnh khi đặt tên cho chúng.
Khi bạn cảm thấy nó có thể gây ra sự rắc rối, ta có thể để chúng trong các class, function hoặc namespaces. Khi mọi thứ vẫn không ổn, trường hợp cuối cùng hãy sử dụng tiền tố.

Hãy tưởng tượng nếu chúng ta có các biến _firstName_ , _lastName_ , _street_ , _houseNumber_ , _city_ , _state_ , và _zipcode_, khi kết hợp với nhau chúng rõ ràng là 1 địa chỉ. Nhưng nếu chỉ thấy biến _state_ thì sao? bạn có biết nó là 1 phần của địa chỉ không?
Ta có thể thêm ngữ cảnh bằng cách sử dụng tiền tố: addrFirstName, addrLastName, addrState,… Ít nhất người đọc sẽ hiểu rằng những biến này là một phần của địa chỉ. Tất nhiên, tốt nhất ta nên có 1 class Address chứa các thuộc tính này.

Ta đi tới 1 vd tiếp theo:

```
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    } else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
    } else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    String guessMessage = String.format("There %s %s %s%s", verb, number, candidate, pluralModifier);
    print(guessMessage);
}
```

Hàm trên khá là dài và các biến được sử dụng xuyên suốt. Thay vào đó, ta có thể tách nhỏ các hàm đi và cho nó và 1 class.

```
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;
    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format("There %s %s %s%s",verb, number, candidate, pluralModifier );
    }
    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }
    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }
    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```

## Vài lời cuối

Thật là khó khi chọn được 1 cái tên chuẩn, điều này đòi hỏi bạn là 1 người có kinh nghiệm lập trình cũng như 1 nền tảng ngôn ngữ tốt. Đây là vấn đề lớn, vì vậy hãy đọc nhiều hơn nữa, suy nghĩ, suy luận nhiều hơn để tìm được chân lý của mình.

Mọi người cũng có xu hướng sợ đổi tên vì sợ làm mọi thứ rắc rối hoặc phật ý người khác. Không có câu trả lời chính xác cho bạn, nhưng hãy để cho mình là 1 tâm hồn đẹp, 1 trái tim nóng và 1 cái đầu lạnh.

Chúng ta phải cảm ơn sâu sắc những người đổi tên khác theo hướng tốt hơn.
Đừng để cái tên tồi tệ phá hủy sự nghiệp coder của mình.
