# PTPMPT20211-12
Repo bài tập lớn môn phát triển phần mềm phân tán. 
Hệ thống bán sản phầm giày.

Các chức năng: 
- Đăng nhập/Đăng ký tài khoản
- Kiểm soát thông tin cá nhân
- Xem sản phẩm tổng quan
- Filter, search theo sản phẩm, tag, nhóm, công ty 
- Mua, kiểm soát thông tin đơn hảng
Các công nghệ sử dụng:
- React JS
- NodeJS Express JS
- Redis caching
- MongoDB
- Docker

Thành phần chính trong ứng dụng:
- service app: FrontEnd: ReactJS, Backend: ExpressJS
- service db: MongoDB
- service redis: Redis làm nơi lưu trữ dữ liệu và xử lý dữ liệu realtime, chia các sentinel redis để phục vụ việc phân tán dữ liệu, chia tải và tăng độ tin cậy

Hệ thống có đầy đủ chức năng đăng nhâp, đăng xuất, truy suất, đọc dữ liệu từ các đơn hàng và xử lý thông tin, kiểm soát quyền. 

## Hướng dẫn setup docker theo docker file
### Cấu trúc project:
  1. Front-end (JavaScript):
  - Sử dụng ReactJS làm framework cho lập trình UI/UX
  - axios làm phần xử lý https restful request
  - react-redux làm quản lý stage
  - nginx làm máy chủ proxy cho đầu ra cổng front end
  2. Backend (JavaScript - NodeJS):
  - expressJS sử dụng làm backend xử lý các restful request
  - mongoose hỗ trợ xử lý truy vấn tới database mongodb
  - express-jwt hỗ trợ authentication dựa trên key và đẩy hash vào database
  - cors và body-parser hỗ trợ xử lý phần parse request đầu vào và xử lý lỗi cors kết nối
  3. Database (MongoDB)
  - Có 3 collection dữ liệu chính (user, product và order)
  - Được cài đặt thẳng tại local hoặc có thể sử dụng online thông qua Mongo Atlas (đã setup sẵn)
  
### Hướng dẫn cài đặt docker và chạy docker
1. Setup tại Dockerfile tại backend folder
- FROM node:lts-alpine : lấy phiên bản nodejs long term support mới nhất
- ENV NODE_ENV=production : môi trường production
- ENV PORT=5000, .... : các env setting để code có thể đọc được cấu hình env
- EXPOSE 5000 : cổng đầu ra của image
- CMD npm start: câu lệnh chạy code trong image

2. Setup tại Dockerfile tại frontend folder
- FROM node:lts-alpine AS package: lấy phiên bản nodeJS long term support mới mất 
- ENV NODE_ENV=production : môi trường production
- RUN yarn : câu lệnh chạy trong image
- FROM node:lts AS build : lấy phiên bản nodeJS mới mất cho build version
- RUN yarn build : câu lệnh build frontend
- FROM nginx:latest : lấy phiên bản nginx mới mất
- RUN rm /etc/nginx/conf.d/default.conf : chạy nginx theo config

3. Setup trong docker-compose (chi tiết trong docker-compose.yml)
- Có 3 image: frontend, backend, mongodb lần lượt phục vụ cho frontend, backend và database
- Chú ý các tham số: images, port, environment, link phục vụ lần lượt cho việc đặt tên image, mapping các cổng đầu ra từ trong image ra ngoài, enviroment phục vụ develop hay production và link cho phép image kết nối trong nội tại.


### Chạy câu lệnh sau để hoàn thành chạy compose container theo setup ở trên: 
```
docker-compose up
```
Mở trình duyệt và đi tới địa chỉ 
```
localhost:3000
```

Ta sẽ thấy có 3 docker image được khởi tạo lần lượt là frontend, backend, mongodb. Frontend được map từ cổng 80 ra ngoài cổng 3000, backend được giữ nguyên từ cổng 5000, database mongodb được giữ nguyên từ cổng 27017. Chú ý, có thể dùng mongo Atlas đã được setup sẵn bằng cách sử dụnng url đã được comment sẵn ở trên trong backend/infrastructure/db.js, có thể thay thế bằng database mong muốn tương ứng.
