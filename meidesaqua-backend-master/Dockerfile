# Escolhe uma imagem base 
FROM node:22.15.0

# Define o diretório de trabalho dentro do container
WORKDIR /app

# Copia os arquivos de dependências primeiro (para aproveitar cache)
COPY package*.json ./

# Instala as dependências
RUN npm install

# Copia o restante do código
COPY . .

# Expõe a porta em que a aplicação vai rodar
EXPOSE 3000

# Define o comando para iniciar o app
CMD ["npm", "start"]