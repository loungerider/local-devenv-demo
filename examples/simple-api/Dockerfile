FROM golang:1.22-alpine as build
WORKDIR /src
COPY main.go .
COPY go.mod .
COPY go.sum .
RUN go mod download && go mod verify

RUN go build -o /app/simple-api ./main.go

FROM alpine:latest
WORKDIR /app
COPY --from=build /app/simple-api .
CMD ["/app/simple-api"]