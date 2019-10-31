# CatalogSource Test

## Preparation
1. A OpenSift cluster or minukube
2. A `CatalogSource` image
3. `CatalogSource` and `Subscription` yaml objects

## CatalogSource Test Images
Image: [docker.io/dinhxuanvu/catsrc-test](https://hub.docker.com/r/dinhxuanvu/catsrc-test)
Tag:
1. :new
2. :old
3. :latest

### How to use `catsrc-test` images:
The `docker.io/dinhxuanvu/catsrc-test:old` is supposed to be the current version `catsrc-test` image. It contains etcd operator for version 0.6.2 and 0.9.0. The `docker.io/dinhxuanvu/catsrc-test:new` is supposed to be the new version `catsrc-test` image. It contains etcd operator for version 0.6.2, 0.9.0, 0.9.2 and 0.9.4.

For testing, tag `:old` version to `:latest` and push the `:latest` tag. The latest image is referenced in `CatalogSource` yaml file. After `CatalogSource` is deployed and running successfully, tag `:new` version to `:latest` and push to the `:latest` tag. Now the `:latest` image is updated. Repeat the process if you want to retest.

## Steps to test new `CatalogSource` polling
1. Create a `CatalogSource` using a current registry image (e.g. `catsrc-test:latest`)
2. Create a `Subscription` pointed to above `CatalogSource` to deploy an operator
3. Push a new image to replace the current `catsrc-test:latest` (ideally the new image would contain a new version of deployed operator)
4. Observe CatalogSource pod to see if it is updated properly.
5. Then, the new version of the operator will be available and `Subscription` should detect it and deploy the new version.

## Command steps:
Need a minikube cluster with latest version of OLM (including the polling feature).
1. `oc create ns test-operators`
2. `oc apply -f operatorgroup.yaml -n test-operators`
3. `oc apply -f catalogsource.yaml -n test-operators`
4. `oc apply -f subscription.yaml -n test-operators`
