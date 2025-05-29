FROM node:18-alpine
WORKDIR /app
COPY frontend/ ./
RUN npm install
EXPOSE 5000
CMD ["npm", "run", "dev", "--", "--host"]
