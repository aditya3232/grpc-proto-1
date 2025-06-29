## Persiapan instalasi protolint & ekstensions vscode

- download protolint : ***https://github.com/yoheimuta/protolint/releases/tag/v0.50.4***
- setelah download letakkan di folder protolint di C
- lalu ekstrak, kemudian daftarkan di environment variables -> system variables -> edit path -> new ***D:\Development\protolint***
- cek dengan buka cmd lalu ketik ***protolint***
- pasang juga protolint di ekstension vscode
- pasang vscode-proto3

## Persiapan instalasi protobuf compiler

- search google : download protocol buffers compiler
- lalu klik link : ***https://github.com/protocolbuffers/protobuf/releases/tag/v27.2***
- lalu ekstrak, kemudian daftarkan di environment variables -> system variables -> edit path -> new ***D:\Development\protoc\bin***
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
- Tambahkan environment variables folder tempat protoc-gen-grpc-gateway.exe berada : environment variables -> system variables -> edit path -> new ***D:\Development\protoc\bin***
- Cek dengan perintah ***protoc-gen-grpc-gateway --version***
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
- Tambahkan environment variables folder tempat protoc-gen-openapiv2.exe berada : environment variables -> system variables -> edit path -> new ***D:\Development\protoc\bin***
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
```bash
## Penjelasan Run Generate Proto to Go GRPC

Perintah protoc ini digunakan untuk mengenerate kode Go dari file .proto. Terdapat dua perintah yang masing-masing menggenerate kode Go biasa dan kode gRPC untuk Go. Berikut adalah penjelasan per baris perintah tersebut:

### (Perintah Pertama: Generate Kode Go Biasa) protoc --go_opt=module=github.com/aditya3232/grpc-proto-1 --go_out=. ./proto/user/*.proto 
- `protoc` adalah compiler untuk file .proto.
- `--go_opt=module=github.com/aditya3232/grpc-proto-1` menentukan module Go yang digunakan. Ini diperlukan agar path impor yang dihasilkan sesuai dengan struktur module di proyek Go.
- `--go_out=.` menentukan direktori output untuk file yang dihasilkan oleh plugin go. Titik (.) menunjukkan bahwa output akan disimpan di direktori saat ini.
- `./proto/user/*.proto` menunjukkan semua file .proto dalam direktori ./proto/user/ yang akan diproses oleh protoc untuk menghasilkan kode Go.

### (Perintah Kedua: Generate Kode gRPC untuk Go) protoc --go-grpc_opt=module=github.com/aditya3232/grpc-proto-1 --go-grpc_out=. ./proto/user/*.proto
- `protoc` adalah compiler untuk file .proto.
- `--go-grpc_opt=module=github.com/aditya3232/grpc-proto-1` menentukan module Go yang digunakan untuk kode gRPC. Ini diperlukan agar path impor yang dihasilkan sesuai dengan struktur module di proyek Go.
- `--go-grpc_out=.` menentukan direktori output untuk file yang dihasilkan oleh plugin go-grpc. Titik (.) menunjukkan bahwa output akan disimpan di direktori saat ini.
- `./proto/user/*.proto` menunjukkan semua file .proto dalam direktori ./proto/user/ yang akan diproses oleh protoc untuk menghasilkan kode gRPC untuk Go.

Dengan menggunakan kedua perintah ini, protoc akan mengenerate dua set file:
- Kode Go Biasa: File yang dihasilkan oleh plugin go akan berisi tipe data dan fungsi dasar yang digunakan untuk memanipulasi data yang didefinisikan dalam file .proto.
- Kode gRPC untuk Go: File yang dihasilkan oleh plugin go-grpc akan berisi kode yang spesifik untuk implementasi gRPC, termasuk server dan client stubs untuk layanan yang didefinisikan dalam file .proto.

Dengan demikian, semua file .proto dalam direktori ./proto/user/ akan diproses untuk menghasilkan kode Go dan kode gRPC untuk Go di direktori saat ini (.).
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
```bash
## Penjelasan Perintah protoc-gen-grpc-gateway

- Perintah protoc yang diberikan digunakan untuk mengenerate kode dari file .proto menggunakan grpc-gateway, yang mengubah definisi layanan gRPC menjadi RESTful API. Berikut adalah penjelasan per baris perintah tersebut:

### protoc -I .
- protoc adalah compiler untuk file .proto.
- `I .` menentukan direktori pencarian untuk file impor. Dalam hal ini, direktori saat ini (.) digunakan.

### --grpc-gateway_out ./protogen/gateway/go
- `--grpc-gateway_out` menentukan direktori output untuk file yang dihasilkan oleh grpc-gateway.
- `./protogen/gateway/go` adalah direktori di mana file yang dihasilkan akan disimpan.

### --grpc-gateway_opt logtostderr=true
- `--grpc-gateway_opt` digunakan untuk memberikan opsi tambahan kepada grpc-gateway.
- `logtostderr=true` mengarahkan grpc-gateway untuk mencatat log ke stderr.

### --grpc-gateway_opt paths=source_relative
- `paths=source_relative` memastikan bahwa jalur file yang dihasilkan adalah relatif terhadap lokasi file .proto.

### --grpc-gateway_opt grpc_api_configuration=./grpc-gateway/config.yml
- `grpc_api_configuration=./grpc-gateway/config.yml` menunjukkan file konfigurasi tambahan untuk grpc-gateway yang berisi pengaturan API gRPC.

### --grpc-gateway_opt standalone=true
- `standalone=true` menghasilkan kode yang dapat berdiri sendiri tanpa perlu server gRPC di latar belakang.


### --grpc-gateway_opt generate_unbound_methods=true
- `generate_unbound_methods=true` menginstruksikan grpc-gateway untuk menghasilkan metode yang tidak terikat, memungkinkan panggilan ke metode gRPC tanpa definisi eksplisit dalam file .proto.

### ./proto/user/*.proto
- Menunjukkan semua file .proto dalam direktori ./proto/user/ yang akan diproses oleh protoc.

Dengan menggunakan perintah ini, protoc akan memproses semua file .proto dalam direktori ./proto/user/, menggunakan konfigurasi yang disediakan, dan menghasilkan kode yang diperlukan di direktori ./protogen/gateway/go.

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
```bash
## Penjelasan Perintah protoc-openapiv2-gateway

Perintah protoc ini digunakan untuk mengenerate spesifikasi OpenAPI v2 dari file .proto menggunakan plugin openapiv2. Berikut adalah penjelasan per baris perintah tersebut:

### protoc -I .
- `protoc` adalah compiler untuk file .proto.
- `-I` . menentukan direktori pencarian untuk file impor. Dalam hal ini, direktori saat ini (.) digunakan.

### --openapiv2_out ./protogen/gateway/openapiv2
- `--openapiv2_out` menentukan direktori output untuk file yang dihasilkan oleh plugin openapiv2.
- `./protogen/gateway/openapiv2` adalah direktori di mana file spesifikasi OpenAPI v2 yang dihasilkan akan disimpan.

### --openapiv2_opt logtostderr=true
- `--openapiv2_opt` digunakan untuk memberikan opsi tambahan kepada plugin openapiv2.
- `logtostderr=true` mengarahkan plugin openapiv2 untuk mencatat log ke stderr.

### --openapiv2_opt output_format=yaml
- `output_format=yaml` menentukan format output sebagai YAML. Secara default, output akan dalam format JSON.

### --openapiv2_opt grpc_api_configuration=./grpc-gateway/config.yml
- `grpc_api_configuration=./grpc-gateway/config.yml` menunjukkan file konfigurasi tambahan untuk grpc-gateway yang berisi pengaturan API gRPC.

### --openapiv2_opt openapi_configuration=./grpc-gateway/config-openapi.yml
- `openapi_configuration=./grpc-gateway/config-openapi.yml` menunjukkan file konfigurasi tambahan untuk openapiv2 yang berisi pengaturan OpenAPI.

### --openapiv2_opt generate_unbound_methods=true
- `generate_unbound_methods=true` menginstruksikan plugin openapiv2 untuk menghasilkan metode yang tidak terikat, memungkinkan panggilan ke metode gRPC tanpa definisi eksplisit dalam file .proto.

### --openapiv2_opt allow_merge=true
- `allow_merge=true` memungkinkan penggabungan beberapa file .proto menjadi satu file spesifikasi OpenAPI.

### --openapiv2_opt merge_file_name=merged
- `merge_file_name=merged` menentukan nama file output hasil penggabungan. Dalam hal ini, file output akan diberi nama merged.

### ./proto/user/*.proto
- Menunjukkan semua file .proto dalam direktori ./proto/user/ yang akan diproses oleh protoc.

Dengan menggunakan perintah ini, protoc akan memproses semua file .proto dalam direktori ./proto/user/, menggunakan konfigurasi yang disediakan, dan menghasilkan spesifikasi OpenAPI v2 dalam format YAML di direktori ./protogen/gateway/openapiv2, dengan nama file merged.yaml.

```

### Install Dependency

```bash
go mod tidy
```

### Run Program (server/ gateway)

```bash
go run cmd/main.go
```