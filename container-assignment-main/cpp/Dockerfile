# Stage 1: Build
FROM gcc:13.2 AS builder
WORKDIR /app
COPY main.cpp .
RUN g++ -O2 -static -o hello main.cpp

# Stage 2: Run
FROM gcr.io/distroless/static-debian11
WORKDIR /app
COPY --from=builder /app/hello .
CMD ["./hello"]
