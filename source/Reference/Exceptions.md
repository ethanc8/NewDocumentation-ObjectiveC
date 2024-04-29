% SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception WITH Swift-exception
# Exceptions

> **Warning**: Exceptions may cause memory leaks! They cause unwinding of the stack, and important method calls, such as `retain` and `release`, may be skipped if the exception occured before then.