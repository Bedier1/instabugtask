# Stage 1
FROM golang:1.16-alpine AS build
WORKDIR /app
RUN adduser \
  --disabled-password \
  --gecos "" \
  --home "/nonexitstent" \
  --shell "/sbin/nologin" \
  --no-create-home \
  --uid "1002" \
  "goapp"
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o myapp

# Stage 2
FROM scratch
# run non root container
COPY --from=build /etc/passwd /etc/passwd
COPY --from=build /etc/group /etc/group
USER goapp
WORKDIR /app
COPY --from=build /app/myapp .
CMD ["./myapp"]

