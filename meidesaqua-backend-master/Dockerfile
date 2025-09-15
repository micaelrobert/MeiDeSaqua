# ESTÁGIO 1: Compilação (Build)
# Usamos uma imagem oficial do Maven que já inclui o Java 24 (JDK)
FROM maven:3-eclipse-temurin-24 AS build

# Define o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copia o 'coração' do projeto primeiro. Se ele не mudar, o Docker usa o cache, acelerando o build.
COPY pom.xml .

# Baixa todas as dependências do projeto de uma vez
RUN mvn dependency:go-offline

# Copia o restante do código-fonte
COPY src ./src

# Compila o projeto, gera o pacote .jar e pula os testes para acelerar o processo de build.
RUN mvn package -DskipTests

# ESTÁGIO 2: Execução (Runtime)
# Usamos uma imagem "alpine", que é extremamente leve e contém apenas o necessário para rodar Java (JRE)
FROM eclipse-temurin:24-jre-alpine

# Define o diretório de trabalho
WORKDIR /app

# Expõe a porta padrão que o Spring Boot utiliza para se comunicar
EXPOSE 8080

# Copia o arquivo .jar que foi gerado no estágio 'build' para este novo contêiner.
# O Docker encontrará o .jar dentro da pasta /app/target/
COPY --from=build /app/target/*.jar app.jar

# Comando final que será executado quando o contêiner for iniciado
ENTRYPOINT ["java", "-jar", "app.jar"]