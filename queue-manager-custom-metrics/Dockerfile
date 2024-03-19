ARG mqSrcImage="icr.io/ibm-messaging/mq"

ARG mqVersion="9.3.3.1-r1"

ARG targetRegistry="image-registry.openshift-image-registry.svc:5000/mq-demo"

# FROM ${targetRegistry}/mq-metrics:${mqVersion} as mq-metrics

FROM ${mqSrcImage}:${mqVersion}


USER 0

RUN cat /etc/os-release && echo "Architecture is: " && arch


### Add support for go:
# /usr/lib64/libc.so.6: version `GLIBC_2.32' not found
RUN rpm -qa

RUN rpm --allfiles -ivh glibc-2.39.9000-8.fc41.src.rpm
# glibc-2.32-32.mga8.x86_64.rpm

# RUN apt-get update && apt-get upgrade -y
# RUN apt-get install libssl-dev

# RUN microdnf --disableplugin=subscription-manager install openssl-devel aaa_glibc-solibs
# RUN microdnf --disableplugin=subscription-manager clean all

# So yes, you need to install aaa_glibc-solibs.
# CGO_ENABLED=0

# COPY --from=mq-metrics /usr/lib64/libgcc_s.so.1 /usr/lib64/libgcc_s.so.1
# COPY --from=mq-metrics /usr/lib64/libstdc++.so.6 /usr/lib64/libstdc++.so.6
# COPY --from=mq-metrics /usr/lib64/libz.so.1 /usr/lib64/libz.so.1


#### Install new metrics support:
RUN mkdir -p /opt/mqm/metrics

# COPY --from=mq-metrics /go/bin/mq_prometheus /usr/local/bin/mqgo/mq_prometheus

# RUN chmod 770 /usr/local/bin/mqgo/mq_prometheus

# COPY --from=mq-metrics /go/src/github.com/ibm-messaging/mq-metric-samples/mq_prometheus.sh /opt/mqm/metrics

RUN chmod 770 /opt/mqm/metrics/*

USER 1001
