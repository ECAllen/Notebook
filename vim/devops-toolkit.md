
# Book notes Devops 2.0 tollkit Viktor Farcic

## Ch. Implementation Breakthrough

CI - integrating, building, and testing code within the developement environment. Usually a couple times a day. 
	push code to repo
	static analysis
	pre-deployment testing
	package and deploy to test env
	pot-deployment testing

Highest priority should be keeping the CI pipeline green.

test phases 

static analysis -> pre-deployment tests -> packaging -> deploy to QA -> post dployment tests

pre-dployment tests - tests that do not need to be deployed to the server to run. i.e. unit tests

feature toggles - essential to disable features when some services are not ready for dployment in CD pipeline

## Ch. System Architecture

SOA 
	boundaries are explicit
	services are autonomous
	services share schema and contract but no class
	service compatibility is based on policy

microservices - think unix utilities and pipes, downside- operational complexity and RPC speed



