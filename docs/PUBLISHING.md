# Publishing

To build the images from the local source code:

2. Install all deps: `pnpm i`
3. Generate types: `pnpm graphql:generate`
4. Build source code: `pnpm build`

or simply `pnpm i && pnpm graphql:generate && pnpm build`

5. Set env vars:

```bash
export COMMIT_SHA="ipv-fix"
export RELEASE="ipv-fix"
export BRANCH_NAME="ipv-fix"
export BUILD_TYPE="publish"
export DOCKER_TAG=":ipv-fix"
export DOCKER_REGISTRY="registry.cistec.com/hive/"
```

6. Compile a local Docker image by running: `docker buildx bake -f docker/docker.hcl publish --load`
7. Use Docker Compose to run the built containers (based on `community` compose file), along with
   the extra containers:

```bash
export DOCKER_TAG=":ipv-fix"
export DOCKER_REGISTRY="registry.cistec.com/"

# composition
docker push registry.cistec.com/hive/composition-federation-2:ipv-fix

# schema
docker push registry.cistec.com/hive/schema:ipv-fix

# tokens
docker push registry.cistec.com/hive/tokens:ipv-fix

# usage-ingestor
docker push registry.cistec.com/hive/usage-ingestor:ipv-fix

# usage
docker push registry.cistec.com/hive/usage:ipv-fix

# webhooks
docker push registry.cistec.com/hive/webhooks:ipv-fix

# server
docker push registry.cistec.com/hive/server:ipv-fix

# app
docker push registry.cistec.com/hive/app:ipv-fix

# storage
docker push registry.cistec.com/hive/storage:ipv-fix

# emails
docker push registry.cistec.com/hive/emails:ipv-fix
```

8. Wait for pipeline to build apollo-router, then pull the image and push it to
   `registry.cistec.com`

```bash
docker pull http://gitlab.cistec.com:5050/core/supergraph/graphql-hive/apollo-router:ipv-fix
docker tag http://gitlab.cistec.com:5050/core/supergraph/graphql-hive/apollo-router:ipv-fix registry.cistec.com/hive/apollo-router:ipv-fix
docker push registry.cistec.com/hive/apollo-router:ipv-fix
```
