FROM dataelement/bisheng-backend:base.v2

WORKDIR /app

COPY ./ ./

RUN pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

ENV DEBIAN_FRONTEND=noninteractive \
    POETRY_REPOSITORIES_ALIYUN_URL="https://mirrors.aliyun.com/pypi/simple/" \
    PIP_INDEX_URL="https://mirrors.aliyun.com/pypi/simple/" \
    PIP_TRUSTED_HOST="mirrors.aliyun.com"


# Upgrade pip and install poetry
RUN pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple
RUN pip install poetry -i https://pypi.tuna.tsinghua.edu.cn/simple

# Configure pip and poetry to use Aliyun mirrors
RUN pip config set global.index-url ${PIP_INDEX_URL} && \
    pip config set global.trusted-host ${PIP_TRUSTED_HOST}

RUN poetry source add --priority=primary aliyun https://mirrors.aliyun.com/pypi/simple/
RUN poetry config repositories.aliyun https://mirrors.aliyun.com/pypi/simple/


RUN poetry config virtualenvs.create false
RUN poetry update --without dev -vv

# patch langchain-openai lib. remove this when upgrade langchain-openai
RUN patch -p1 < /app/bisheng/patches/langchain_openai.patch /usr/local/lib/python3.10/site-packages/langchain_openai/chat_models/base.py

# patch fastapi-jwt-auth lib. remove this when remove fastapi-jwt-auth
# fix fastapi-jwt-auth not support pydantic:v2
RUN patch -p1 < /app/bisheng/patches/fastapi_jwt_auth.patch /usr/local/lib/python3.10/site-packages/fastapi_jwt_auth/config.py

CMD ["sh entrypoint.sh"]
