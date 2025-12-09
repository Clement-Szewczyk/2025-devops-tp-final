FROM node:24-alpine AS build-stage

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM nginx:alpine 

# Copier les fichiers buildés
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Copier le template dans le dossier templates (Nginx appliquera envsubst automatiquement)
COPY nginx.conf.template /etc/nginx/templates/default.conf.template



EXPOSE 5173

CMD ["nginx", "-g", "daemon off;"]