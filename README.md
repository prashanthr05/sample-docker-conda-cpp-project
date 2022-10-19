## Hello World Cpp with Docker and Conda

A template repository for Docker and Conda based C++ Projects with a DevSecOps mindset.


### Building the docker image

With the Docker daemon running, From within the `dockerfiles` folder, run

```
docker image build --file Dockerfile --tag sample-cpp-conda:latest ..
```

`..` specifies the root directory as the context.



### Running the docker image

Once the image is built,

```
docker run -it sample-cpp-conda:latest
```

We will be taken into the `sample-cpp` project folder inside the container with a Conda-based virtual environment `myenv` activated containing necessary basic packages installed for C++ development.

To compile the sample HelloWorld project from within the container,

```
mkdir build
cd build
cmake ..
make
```

### Current Issues

- [ ] Running cmake causes linking error unable to find gcc on Ubuntu 22.04



### Possible Improvements

- [ ] Expose volume for  project's workspace instead of copying source code into the folder
- [ ] Use Docker Compose instead of long docker command line instructions
- [ ] Add CI for docker image build and source code build
- [ ] Add CMake skeleton for creating library
- [ ] Add example of python bindings support
- [ ] Add VSCode IDE support
- [ ] Add GUI support inside the container
- [ ] Add GPU support
- [ ] Add source code formatting and linter support
- [ ] Add testing support
- [ ] Add development phase security checks



### References

- [cpp-conda](https://github.com/btjanaka/cpp-conda)
- [python-data-science-project](https://github.com/KAUST-Academy/python-data-science-project)

- [devenv](https://github.com/diegoferigo/devenv)
