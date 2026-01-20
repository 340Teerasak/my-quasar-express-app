# Stage 1: Build Quasar SPA
FROM node:20-alpine AS builder

WORKDIR /app

# Copy config และ package ก่อนเพื่อ cache dependencies ดีขึ้น
COPY quasar.config.* ./
COPY package*.json ./
COPY .npmrc ./     
# ถ้ามี .npmrc สำหรับ private packages

# Install deps
RUN npm ci --legacy-peer-deps   
# ใช้ --legacy-peer-deps ถ้ามี peer deps conflict ใน Quasar v2

# Copy โค้ดที่เหลือ (src, public, etc.)
COPY . .

# Build production bundle (ใช้ npx เพื่อเรียก quasar CLI จาก node_modules)
RUN npx quasar build -m spa

# Stage 2: Serve ด้วย nginx (ขนาดเล็กมาก)
FROM nginx:alpine AS production-stage

# Copy built files จาก builder stage
COPY --from=builder /app/dist/spa /usr/share/nginx/html

# Optional: เพิ่ม custom nginx config ถ้าต้องการ (เช่น proxy API)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]