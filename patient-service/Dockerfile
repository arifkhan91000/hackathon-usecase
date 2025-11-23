# Node multi-stage image (small final image)
FROM node:18-alpine AS builder
WORKDIR /app

# copy package files and install production deps
COPY package*.json ./
RUN npm install --production

# copy source
COPY . .

# optional: build step if you have a build script
# RUN npm run build

# final lightweight image
FROM node:18-alpine
WORKDIR /app

# copy production node_modules and app files
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app ./

# expose the port your app listens on
ENV PORT=8080
EXPOSE 8080

# start command: change 'index.js' if your app path differs (e.g., src/index.js)
CMD ["node", "src/index.js"]
