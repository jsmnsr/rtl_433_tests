# ELRO AB440R Flex symbol-copy regression

This synthetic 451-pulse OOK input exercises the shipped `elro_ab440r.conf`
Flex decoder. Gaps create 50 bitbuffer rows, with a long final row that once
caused `flex_callback()` to pass a decoded bit count to `memcpy()` as bytes.
The resulting copy ran beyond the bitbuffer and terminated the process on the
unpatched build. See [rtl_433 PR #3715](https://github.com/merbanan/rtl_433/pull/3715).

The expected JSON records successful decoding. The `require_clean_exit`
marker also checks the process exit status: the unpatched build emits JSON
before terminating on a stack protector, so comparing output alone misses it.
This is a generated boundary test, not a recording of a physical remote.
