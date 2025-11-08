FROM envoyproxy/envoy:v1.31-latest
COPY envoy.yaml /etc/envoy/envoy.yaml
EXPOSE 8080 9901
ENTRYPOINT ["envoy", "-c", "/etc/envoy/envoy.yaml", "--service-node", "gateway", "--service-cluster", "local"]
