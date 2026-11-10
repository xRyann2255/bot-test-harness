# bot test harness

Runs test games for a game-playing bot on GitHub Actions, one batch per manually dispatched run.

- Every batch is ciphertext (OpenPGP, AES-256), uploaded as a release asset and deleted after use. Its key is the
  one secret of a one-off environment, deleted with the run once the result is collected.
- What every batch of a test run shares goes up once, as one more ciphertext release asset under its own run key, deleted
  when the run ends. That key is never a repository secret: a job gets it only inside its own encrypted batch.
- The key is read only inside new network, mount, PID, IPC and UTS namespaces (loopback only, host sockets and swap out of reach,
  a private tmpfs for every decrypted byte); the games run as an unprivileged user. The result is encrypted before it
  leaves and kept as a 1-day artifact. The log shows fixed lines.
- The workflow runs only on manual dispatch, has no token permissions and uses one GitHub action, pinned by hash.

Public metadata: when runs happen, how long they take, and the sizes of the encrypted files.
