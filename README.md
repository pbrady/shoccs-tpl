# shoccs-tpl

Simple cmake wrapper to download and build the collection of third party libraries required to build [shoccs](https://github.com/lanl/shoccs).  Building should be a straightforward process of

```shell
  $ cd /path/to/cloned/repo
  $ cmake -Bbuild -H. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=...
  $ cd build && make install
```
