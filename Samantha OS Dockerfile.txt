FROM n8nio/n8n:latest

# Install additional dependencies if needed
USER root
RUN apk add --update --no-cache \
    postgresql-client

# Switch back to node user
USER node

# Set working directory
WORKDIR /home/node

# Environment variables (will be overridden by Render)
ENV N8N_PORT=5678
ENV N8N_PROTOCOL=https
ENV N8N_BASIC_AUTH_ACTIVE=true
ENV N8N_METRICS=false
ENV EXECUTIONS_DATA_PRUNE=true
ENV EXECUTIONS_DATA_MAX_AGE=168

# Expose port
EXPOSE 5678

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:5678/healthz || exit 1

# Start N8N
CMD ["n8n"]
