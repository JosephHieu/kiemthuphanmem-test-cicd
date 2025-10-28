# ----- Stage 1: Build (Xây dựng code React) -----
FROM node:20-alpine AS build

WORKDIR /app

# Copy package.json và package-lock.json (hoặc yarn.lock)
COPY package*.json ./

# Cài đặt dependencies
RUN npm install

# Copy toàn bộ source code frontend
COPY . .

# Build ứng dụng React, tạo ra thư mục /build
RUN npm run build

# ----- Stage 2: Serve (Phục vụ file tĩnh) -----
# Dùng base image Nginx (web server siêu nhẹ)
FROM nginx:1.25-alpine

# Copy các file tĩnh đã build từ Stage 1
# vào thư mục web-root mặc định của Nginx
COPY --from=build /app/build /usr/share/nginx/html

# (Tùy chọn) Nếu bạn có file cấu hình Nginx riêng (ví dụ: để xử lý routing)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose cổng 80 (cổng HTTP mặc định)
EXPOSE 80

# Lệnh mặc định để chạy Nginx
CMD ["nginx", "-g", "daemon off;"]