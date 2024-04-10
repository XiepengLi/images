FROM yidadaa/chatgpt-next-web:v2.11.3 as chat
FROM ghcr.io/aurora-develop/aurora:latest as aurora
FROM searxng/searxng:latest
RUN apk add --no-cache supervisor curl git nodejs 
COPY --from=aurora /app /app
COPY --from=chat /app /app/chatgpt-next-web
RUN git clone --depth=1 https://github.com/nashsu/FreeAskInternet.git /app/free-ask-internet 
RUN cp -r /app/free-ask-internet/searxng /etc/searxng
RUN cd /app/free-ask-internet && python3 -m venv venv && source venv/bin/activate && pip install -r requirements.txt --no-cache-dir
COPY free-ask-internet/supervisord.conf /etc/supervisord.conf
COPY free-ask-internet/supervisor.d /etc/supervisor.d
COPY free-ask-internet/hosts /etc/hosts
EXPOSE 3000
EXPOSE 3040
EXPOSE 8000
ENTRYPOINT []
CMD ["/usr/bin/supervisord","-c","/etc/supervisord.conf"]
