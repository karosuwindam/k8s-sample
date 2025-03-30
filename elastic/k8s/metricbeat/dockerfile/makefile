TAG = 8.15.5
DOCKER = docker
NAME = 192.168.0.91:5000/elastic/metricbeat

BUILD = buildctl
BUILD_ADDR = tcp://192.168.0.92:1234 #arm64
BUILD_OPTION = "type=image,push=true,registry.insecure=true"

TARGET = Dockerfile
TARGET_FILE = ./
ARCH = arm64
OPT = "--privileged"


build-kit: 
	@echo "--- buildkit build --"
	${BUILD} --addr ${BUILD_ADDR} build --output name=${NAME}:${TAG},${BUILD_OPTION} --frontend=dockerfile.v0 --local context=${TARGET_FILE}   --local dockerfile=${TARGET_FILE} --opt source=${TARGET_FILE}${TARGET}
