# ---------- Build Stage ----------
FROM node:18-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Accept build-time args (safe if values are NOT secrets)
ARG REACT_APP_AUTH_API_URL
ARG REACT_APP_STREAMING_API_URL
ARG REACT_APP_STREAMING_PUBLIC_URL
ARG REACT_APP_ADMIN_API_URL
ARG REACT_APP_CHAT_API_URL
ARG REACT_APP_CHAT_SOCKET_URL

# Use only during build (React embeds them at build time)
ENV REACT_APP_AUTH_API_URL=$REACT_APP_AUTH_API_URL
ENV REACT_APP_STREAMING_API_URL=$REACT_APP_STREAMING_API_URL
ENV REACT_APP_STREAMING_PUBLIC_URL=$REACT_APP_STREAMING_PUBLIC_URL
ENV REACT_APP_ADMIN_API_URL=$REACT_APP_ADMIN_API_URL
ENV REACT_APP_CHAT_API_URL=$REACT_APP_CHAT_API_URL
ENV REACT_APP_CHAT_SOCKET_URL=$REACT_APP_CHAT_SOCKET_URL

RUN npm run build


# ---------- Production Stage ----------
FROM nginx:1.27-alpine AS production

# Copy build output
COPY --from=build /app/build /usr/share/nginx/html

# Expose correct nginx port
EXPOSE 3000

# Run nginx (NOT npm)
CMD ["nginx", "-g", "daemon off;"]
