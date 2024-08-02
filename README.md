## Persiapan instalasi protolint & ekstensions vscode

- download protolint : ***https://github.com/yoheimuta/protolint/releases/tag/v0.50.4***
- setelah download letakkan di folder D, jangan di C
- lalu ekstrak, kemudian daftarkan di environment variables -> system variables ***D:\Development\protolint***
- cek dengan buka cmd lalu ketik ***protolint***
- pasang juga protolint di ekstension vscode
- pasang vscode-proto3

## Persiapan instalasi protobuf compiler

- search google : download protocol buffers compiler
- lalu klik link : ***https://github.com/protocolbuffers/protobuf/releases/tag/v27.2***
- lalu ekstrak, kemudian daftarkan di environment variables -> system variables ***D:\Development\protoc\bin***
- cek dengan buka cmd lalu ketik ***protoc***

## Persiapan download protoc-gen-go

- buka terminal dan jalankan
- go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.34.2
- go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.5.1

## Persiapan protoc-gen

- Buat folder proto/{nama data} misal proto/user
- Isi dengan file proto
- Tentukan nama package, option go_package, message req & resp, dan service nya
- Hapus file protogen jika dilakukan compile ulang
- Sebelum jalankan **protoc-gen-grpc-gateway**, buat folder ./protogen/gateway/go/
- Sebelum jalankan **protoc-openapiv2-gateway**, buat folder ./protogen/gateway/openapiv2

## Persiapan protoc-gen-grpc-gateway:

- Download protoc-gen--grpc-gateway dari repo berikut: https://github.com/grpc-ecosystem/grpc-gateway/releases/tag/v2.20.0
- Rename ke protoc-gen-grpc-gateway.exe (windows)
- Tambahkan environment variables folder tempat protoc-gen-grpc-gateway.exe berada : ***D:\Development\protoc\bin***
- Cek dengan perintah ***protoc-gen-grpc-gateway --version***
- Cek dengan protoc-gen-grpc-gateway --version
- Buat folder proto/google/api
- Download google/api/annotations.proto copy filenya dan masukkan ke proto/google/api/annotations.proto
- Lakukan juga google/api/http.protocopy file dan masukkan ke proto/google/api/http.proto
- Buat konfigurasi eksternal grpc-gateway/config.yml
- Pakai protoc-gen-grpc-gateway untuk membuat rest proxy didalam folder protogen/gateway/go
- Jangan lupa buat direktori ini terlebih dahulu ***protogen/gateway/go***
- Hapus protogen/gateway/go jika dilakukan compile ulang

## Persiapan protoc-gen-openapi-v2

- Download protoc-gen-openapiv2 dari repo berikut: https://github.com/grpc-ecosystem/grpc-gateway/releases/tag/v2.20.0
- Rename ke protoc-gen-openapiv2.exe (windows)
- Tambahkan environment variables folder tempat protoc-gen-openapiv2.exe berada : ***D:\Development\protoc\bin***
- Cek dengan perintah ***protoc-gen-openapiv2 --version***
- Buat konfigurasi eksternal grpc-gateway/config-openapi.yml
- Buat folder proto/protoc-gen-openapiv2/options
- Download protoc-gen-openapiv2/options/anntations.proto dan masukkan ke proto/protoc-gen-openapiv2/options/annotations.proto
- Download protoc-gen-openapiv2/options/openapiv2.proto dan masukkan ke proto/protoc-gen-openapiv2/options/openapiv2.proto
- buat folder ini ***protogen/gateway/openapiv2***
- Hapus protogen/gateway/openapiv2 jika dilakukan compile ulang

## install Framework & Library

- Protobuf : google.golang.org/protobuf
- Protobuf GRPC : go get -u google.golang.org/grpc
- Viper (Configuration) : go get github.com/spf13/viper
- Logrus (Logging) : go get github.com/sirupsen/logrus


### Run Generate Proto to Go GRPC

```shell
protoc --go_opt=module={module_name} --go_out=. ./proto/*.proto
protoc --go-grpc_opt=module={module_name} --go-grpc_out=. ./proto/*.proto

# example:
protoc --go_opt=module=github.com/aditya3232/grpc-proto-1 --go_out=. ./proto/user/*.proto
protoc --go-grpc_opt=module=github.com/aditya3232/grpc-proto-1 --go-grpc_out=. ./proto/user/*.proto
```

### protoc-gen-grpc-gateway
```shell
protoc -I . \
    --go_out ./gen/go/ --go_opt paths=source_relative \
    --go-grpc_out ./gen/go/ --go-grpc_opt paths=source_relative \
    your/service/v1/your_service.proto

example:

protoc -I . \
	--grpc-gateway_out ./protogen/gateway/go \
	--grpc-gateway_opt logtostderr=true \
	--grpc-gateway_opt paths=source_relative \
	--grpc-gateway_opt grpc_api_configuration=./grpc-gateway/config.yml \
	--grpc-gateway_opt standalone=true \
	--grpc-gateway_opt generate_unbound_methods=true \
	./proto/user/*.proto \

example single line:

protoc -I . --grpc-gateway_out ./protogen/gateway/go --grpc-gateway_opt logtostderr=true --grpc-gateway_opt paths=source_relative --grpc-gateway_opt grpc_api_configuration=./grpc-gateway/config.yml --grpc-gateway_opt standalone=true --grpc-gateway_opt generate_unbound_methods=true ./proto/user/*.proto

```

### protoc-openapiv2-gateway
```bash
example:

protoc -I . --openapiv2_out ./protogen/gateway/openapiv2 \
	--openapiv2_opt logtostderr=true \
	--openapiv2_opt output_format=yaml \
	--openapiv2_opt grpc_api_configuration=./grpc-gateway/config.yml \
  --openapiv2_opt openapi_configuration=./grpc-gateway/config-openapi.yml \
	--openapiv2_opt generate_unbound_methods=true \
	--openapiv2_opt allow_merge=true \
	--openapiv2_opt merge_file_name=merged \
  ./proto/user/*.proto \

  example single line: 

protoc -I . --openapiv2_out ./protogen/gateway/openapiv2 --openapiv2_opt logtostderr=true --openapiv2_opt output_format=yaml --openapiv2_opt grpc_api_configuration=./grpc-gateway/config.yml --openapiv2_opt openapi_configuration=./grpc-gateway/config-openapi.yml --openapiv2_opt generate_unbound_methods=true --openapiv2_opt allow_merge=true --openapiv2_opt merge_file_name=merged ./proto/user/*.proto


```

### Install Dependency

```bash
go mod tidy
```

### Run Program (server/ gateway)

```bash
go run cmd/main.go
```