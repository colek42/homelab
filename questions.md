• Is having developers make deployments an anti-pattern that needs to be avoided?

Developer team leads are usually in the best position to write these deployments as they are most likely to be
interfacing with ops, and have the most knowledge about the application

• Does this also apply to the helm chart development?

Yes.  Though this will take more coordination with ops.

• If teams use local development environments, is something like Minikube sufficient? Should they be using something else like K8s for Docker or MicroK8s.

For Go applications I will have 2 Dockerfiles.  One of the docker files gets set up as a development docker file
this will include the tooling needed for the application, it will also have tooling to detect changes and recompile the application.  I then mount my GOPATH into the container.  Everytime a change is made to a file, the application gets stopped compiled, tested, and restarted in the container with the new code changes.

I deploy this `dev` container and it's dependents with docker-compose.  Only when I am working on actual Kubernetes manifests do I actually use k8s for development.  K8s can get pretty heavy for a devlaptop running in a VM, I have found that using docker-compose for local dev works great.  If you start hooking into k8s inside of your application you will have to adjust fire.


• Are there tooling considerations for local testing (Mocking, frameworks), etc. as is pertains specifically to Go microservice development. (edited)

- I have never seen a need to move past the included testing framework in Go.
- Your most important tests are going to be database migration tests, and full coverage of your API.
- Know what your test coverage is (go cover)
- Go has a ton of great code qualiy tooling, check out https://github.com/golangci/golangci-lint