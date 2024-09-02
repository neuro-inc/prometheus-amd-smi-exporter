# MIT-X11 Open Source License
# Copyright (c) 2024, Advanced Micro Devices, Inc.
build:
	make clean
	go mod init src
	go get golang.org/x/exp/slices
	go get github.com/prometheus/client_golang
	go get github.com/prometheus/client_golang/prometheus
	go get github.com/prometheus/client_golang/prometheus/promhttp
	go get github.com/amd/go_amd_smi@v1.0.0
	go build -o amd_smi_exporter main.go cpu_data.go

clean:
	rm -rf amd_smi_exporter go.mod go.sum

install:
	sudo cp amd_smi_exporter /usr/local/bin
	sudo cp amd-smi-exporter.service /etc/systemd/system
	sudo cp amd-smi-custom-rules.yml /etc/prometheus

docker_build:
	mkdir -p ./install/library
	mkdir -p ./install/binconfig
	sudo cp -r /opt/goamdsmi ./install/library
	sudo cp -r /opt/e-sms ./install/library
	sudo cp -r /opt/rocm ./install/library
	sudo cp amd_smi_exporter ./install/binconfig
	sudo docker build -t prometheus-amd-smi-exporter:$(TAG) .
	sudo docker save -o k8/prometheus-amd-smi-exporter.tar prometheus-amd-smi-exporter:$(TAG)

docker_push:
	sudo docker push $(REGISTRY)/$(IMAGE_NAME):$(TAG)
	
container_clean:
	sudo rm -rf install
	sudo rm -rf k8/amd_smi_exporter_container.tar
	sudo docker rmi amd_smi_exporter_container:$(TAG)
