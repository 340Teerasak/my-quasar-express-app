# Stage 1: Build the Quasar app
FROM node:20-alpine AS builder

# ตั้ง working directory ให้ชัดเจน
WORKDIR /app

# Copy package files ก่อน เพื่อ cache dependencies
COPY package*.json ./
COPY quasar.config.* ./?   
# Install dependencies
RUN npm ci   # หรือ npm install ถ้า ci ไม่ work

# Copy โค้ดทั้งหมด
COPY . .

# Build Quasar app (สำคัญ: run ภายใน /app ซึ่งเป็น Quasar project)
RUN npx quasar build -m spa   # หรือ npm run build ถ้า script ใน package.json เป็น "build": "quasar build -m spa"

# Stage 2: Serve with nginx
FROM nginx:alpine

# Copy built files จาก stage แรกไปยัง nginx
COPY --from=builder /app/dist/spa /usr/share/nginx/html

# Expose port
EXPOSE 80

# Run nginx
CMD ["nginx", "-g", "daemon off;"]