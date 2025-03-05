FROM golang:1.23-alpine AS builder

WORKDIR /app

RUN apk add --no-cache git

# Copy go.mod and go.sum to the working directory
COPY Day1/2.crd/go.mod Day1/2.crd/go.sum ./
RUN go mod tidy

# Copy the main.go file to the working directory
COPY Day1/2.crd/main.go ./

# Download additional dependencies
RUN go get -d ./...

# Build the Go application
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o secret-operator main.go

FROM alpine:latest

WORKDIR /root/

# Copy the built application from the builder stage
COPY --from=builder /app/secret-operator .

# Make the application executable
RUN chmod +x secret-operator

# Set the command to run the application
CMD ["./secret-operator"]
