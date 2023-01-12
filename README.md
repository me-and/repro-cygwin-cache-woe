GitHub's [@actions/cache][] library, used by the [actions/cache][] action, is
[hardcoded to use Git for Windows' `tar`][hardcode].  That's a problem if you
install [Cygwin][] and put Cygwin's binaries in the PATH, because the Git for
Windows includes older versions of the Cygwin libraries, GfW `tar` expects to
be linked against those, but actually finds the freshly installed Cygwin DLLs,
and promptly falls over.

[@actions/cache]: https://github.com/actions/toolkit/blob/main/packages/cache
[actions/cache]: https://github.com/actions/cache
[hardcode]: https://github.com/actions/toolkit/blob/6c1f9eaae833355a0b212b66c5f2e3ac366de185/packages/cache/src/internal/constants.ts#L31
[Cygwin]: https://www.cygwin.com/

```
Run actions/cache/save@v3
"C:\Program Files\Git\usr\bin\tar.exe" --posix -cf cache.tzst --exclude cache.tzst -P -C D:/a/cygwin-install-action/cygwin-install-action --files-from manifest.txt --force-local --use-compress-program "zstd -T0"
/usr/bin/tar: cache.tzst: Cannot write: Broken pipe
/usr/bin/tar: Child returned status 127
/usr/bin/tar: Error is not recoverable: exiting now
Warning: Failed to save: "C:\Program failed with error: The process 'C:\Program Files\Git\usr\bin\tar.exe' failed with exit code 2
Warning: Cache save failed.
```

This repository hopefully provides a simple test case to demonstrate the issue.
