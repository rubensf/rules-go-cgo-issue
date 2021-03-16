Have mingw installed, eg

```
$ choco install -y msys2
$ pacman -S --noconfirm mingw
```

Put it on your path. Get bazelisk

```
$ choco install -y bazelisk
```


Then
```
$ bazelisk build //:bin --verbose_failures --execution_log_json_file=out.json
```


Note that these are _all_ empty files:

```
# Go files
package main
func main() {}
# Cpp files
int main() {}
```

You'll get an error eg:
```
ERROR: C:/src/testpr/BUILD.bazel:8:11: Linking liblib2_4a72131949.so failed: (Exit 1): gcc failed: error executing command cd C:/users/rubensf/_bazel_rubensf/rckt63al/execroot/test
  SET PATH=c:/tools/msys64/mingw64/bin
  SET PWD=/proc/self/cwd
  SET RUNFILES_MANIFEST_ONLY=1
  c:/tools/msys64/mingw64/bin/gcc -shared -o bazel-out/x64_windows-fastbuild/bin/liblib2_4a72131949.so
-Wl,-rpath,$ORIGIN/_solib_x64_windows /../ -Lbazel-out/x64_windows-fastbuild/bin bazel-out/x64_windows-fastbuild/bin/_objs/lib2/lib2.o
-llib1_4a72131949 -Wl,-S -lstdc++
Execution platform: @local_config_platform//:host
c:/tools/msys64/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/10.2.0/../../../../x86_64-w64-mingw32/bin/ld.exe: cannot find -llib1_4a72131949
collect2.exe: error: ld returned 1 exit status
```

Checking the out.json file, you can see that lib1 is correctly resolved
as a dependency, located at `bazel-out/x64_windows-fastbuild/bin/liblib1_4a72131949.so`. You
should be able to see it locally.
