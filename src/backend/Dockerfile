FROM dataelement/bisheng-backend:base.v2

WORKDIR /app

COPY ./ ./

# 配置 pip 镜像源为阿里云
RUN pip config set global.index-url http://mirrors.aliyun.com/pypi/simple/
RUN pip config set install.trusted-host mirrors.aliyun.com
# 设置 Poetry 的镜像源为阿里云
# RUN poetry config repositories.pypi https://mirrors.aliyun.com/pypi/simple/
RUN poetry source add --priority=primary aliyun https://mirrors.aliyun.com/pypi/simple/


RUN poetry config virtualenvs.create false
RUN poetry update --without dev -vv

# patch langchain-openai lib. remove this when upgrade langchain-openai
RUN patch -p1 < /app/bisheng/patches/langchain_openai.patch /usr/local/lib/python3.10/site-packages/langchain_openai/chat_models/base.py

# patch fastapi-jwt-auth lib. remove this when remove fastapi-jwt-auth
# fix fastapi-jwt-auth not support pydantic:v2
RUN patch -p1 < /app/bisheng/patches/fastapi_jwt_auth.patch /usr/local/lib/python3.10/site-packages/fastapi_jwt_auth/config.py

CMD ["sh entrypoint.sh"]
