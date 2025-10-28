# ----- Stage 1: Build (Xây dựng code) -----
# Sử dụng base image Maven để build file .jar
FROM maven:3.8.5-openjdk-17 AS build

# Đặt thư mục làm việc
WORKDIR /app

# Copy file pom.xml và tải dependencies (để tận dụng cache)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy toàn bộ source code backend
COPY src ./src

# Build và đóng gói ứng dụng (bỏ qua test để build nhanh hơn)
RUN mvn clean package -DskipTests

# ----- Stage 2: Run (Chạy ứng dụng) -----
# Sử dụng base image Java 17 siêu nhẹ
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copy file .jar đã build từ Stage 1
# Hãy chắc chắn tên file .jar của bạn khớp (ví dụ: backend-0.0.1-SNAPSHOT.jar)
# Nếu không chắc, hãy dùng 'target/*.jar'
COPY --from=build /app/target/*.jar app.jar

# Cổng mà Spring Boot của bạn đang chạy (ví dụ: 8080)
EXPOSE 8080

# Lệnh để chạy ứng dụng
ENTRYPOINT ["java", "-jar", "app.jar"]