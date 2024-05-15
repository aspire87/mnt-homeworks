FROM centos:7 as builder



RUN yum install gcc openssl-devel bzip2-devel libffi libffi-devel wget make -y && \
    wget https://www.python.org/ftp/python/3.9.18/Python-3.9.18.tgz  && \
    tar -xzf Python-3.9.18.tgz &&  \
    rm Python-3.9.18.tgz && \
    yum clean all &&  \
    rm -rf /var/cache/yum/*

WORKDIR /Python-3.9.18
RUN ./configure --enable-optimizations && \
    make altinstall && \
    cd ../ && rm -rf /Python-3.9.18

FROM centos:7

EXPOSE 5290
COPY --from=builder /usr/local/include/python3.9/ /usr/local/include/python3.9/
COPY --from=builder /usr/local/lib/python3.9/ /usr/local/lib/python3.9/
COPY --from=builder /usr/local/bin/python3.9 /usr/local/bin/python3.9
RUN groupadd --gid 1001 --system app &&  \
    adduser --uid 1001 --gid app --create-home --system app
USER app

WORKDIR /python_api
COPY --chown=app requirements.txt requirements.txt
RUN python3.9 -m pip install -r requirements.txt
COPY --chown=app python-api.py python-api.py

ENTRYPOINT ["python3.9", "python-api.py"]
