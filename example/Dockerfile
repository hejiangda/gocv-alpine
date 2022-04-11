FROM hejiangda/gocv-static-builder:v0.1 as build-stage

RUN /version/gocv-version

FROM alpine

COPY --from=build-stage /version/gocv-version /gocv-version
ENTRYPOINT ["/gocv-version"]
