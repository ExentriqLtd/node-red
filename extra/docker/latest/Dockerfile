ARG NODE_VERSION=6
FROM node:${NODE_VERSION}

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /exentriq
RUN mkdir /exentriq/prod
RUN mkdir /exentriq/stage

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /exentriq \
    && chown -R node-red:node-red /usr/src/node-red

RUN npm install -g when
RUN npm install -g rest

USER node-red

# package.json contains Node-RED NPM module and node dependencies
COPY src/package.json /usr/src/node-red/
COPY src/stage/settings.js /exentriq/stage/
COPY src/stage/exentriq-authentication.js /exentriq/stage/
COPY src/prod/settings.js /exentriq/prod/
COPY src/prod/exentriq-authentication.js /exentriq/prod/

RUN npm install

# User configuration directory volume
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json
ENV DEST=stage
ENV NODE_PATH=/usr/src/node-red/node_modules:/data/node_modules

CMD ["npm", "start", "--", "--userDir", "/data"]
