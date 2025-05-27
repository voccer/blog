Nhân tiện dạo này dự án cần sử dụng timeout cho hàm async trong javascript, mình có tìm hiểu thêm 1 số phương thức trong Promise.

# Promise là gì?

Để bắt đầu thì promise là 1 cơ chế đặc biệt trong javascript, giúp chúng ta thực thi các tác vụ bất đồng bộ. Nhắc tới promise thì ta không thể không nhắc tới callback (nỗi ám ảnh của javascript), promise sinh ra để khắc phục các nhược điểm của callback.

# Các trạng thái của Promise

Promise có 3 trạng thái:

- pending: trạng thái chỉ đuơc khởi tạo, chưa hoàn thành
- fulfilled: hoàn thành và thành công
- rejected: hoàn thành và thất bại

# Các phương thức trong Promise

## Promise.all

Chắc hẳn ai code javascript 1 thời gian có sử dụng async/await đều biết và sử dụng tới phương thức này.
Phương thức này nhận vào 1 mảng các promises và chỉ được hoàn tất khi tất cả các promises `fulfilled` hoặc có 1 promise `rejected`.
Phương thức này dùng khá phổ biến, đặc biệt là ứng dụng trong các job cần các task vụ chạy đồng thời, song song.

ví dụ:

```
async function crawl(url){
  return await crawlFunction(url)
}

async function main(){
  const a = [url1, url2, url3]
  await Promise.all(a.map((url)=>crawl(url)))
}
main()
```

## Promise.race

Phương thức này nhận vào 1 mảng các promises và được hoàn tất khi 1 promises `fulfilled` hoặc `rejected`.
Phương thức này dùng lúc cần chạy 1 hàm
