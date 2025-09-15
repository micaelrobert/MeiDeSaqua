# ---- ESTÁGIO 1: Build ----
# Usamos uma imagem Node.js completa para instalar dependências e construir o projeto.
# A versão '20-alpine' é uma LTS (Long-Term Support), leve e segura.
FROM node:20-alpine AS build

# Define o diretório de trabalho
WORKDIR /app

# Copia os arquivos de manifesto de dependências
COPY package.json ./
COPY package-lock.json ./

# Instala as dependências de produção
RUN npm install --only=production

# Copia o restante do código-fonte
COPY . .

# Constrói a aplicação para produção
RUN npm run build

# ---- ESTÁGIO 2: Produção ----
# Começamos com uma imagem limpa e ainda mais leve.
FROM node:20-alpine

WORKDIR /app

# Copia os arquivos de configuração e a pasta 'public'
COPY --from=build /app/public ./public
COPY --from=build /app/package.json ./package.json
COPY --from=build /app/next.config.mjs ./next.config.mjs

# Copia a pasta '.next' já construída do estágio anterior
COPY --from=build /app/.next ./.next

# Expõe a porta que o Next.js usa
EXPOSE 3000

# O comando para iniciar a aplicação em modo de produção
CMD ["npm", "start"]