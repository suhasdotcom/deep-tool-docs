# 📝 Git Notes: Pushing Notes to Remotes

Git notes allow you to associate additional information with commits without changing the commits themselves. However, by default, Git does not push notes to remotes unless explicitly configured.

## 📤 Pushing Notes

To push notes to a remote, you must specify the `refs/notes/*` ref namespace:

```bash
git push origin refs/notes/*
```

This pushes all notes under the `refs/notes/` namespace to the `origin` remote.

> 💡 If you're using a custom notes namespace (e.g., `refs/notes/review`), you can push only that:
>
> ```bash
> git push origin refs/notes/review
> ```

## 🎯 Pushing Specific Custom Notes Namespaces

To push notes that are scoped to a specific application or concern, such as internal pipelines, you can push individual refs directly:

```bash
git push origin refs/notes/enc-internal/commit/processed
```

Or push multiple specific namespaces:

```bash
git push origin refs/notes/enc-internal/commit/processed refs/notes/enc-internal/commit/rev-processes
```

Or use a wildcard to push all under a common prefix:

```bash
git push origin 'refs/notes/enc-internal/commit/*'
```

> 🧠 Quoting the refspec with wildcards is necessary to avoid shell expansion.

## ⚙️ Persisting Notes Push Configuration

🔗 Related discussion: [https://stackoverflow.com/questions/16921988/is-it-possible-to-get-git-to-automatically-update-notes-on-the-remote-server-whe](https://stackoverflow.com/questions/16921988/is-it-possible-to-get-git-to-automatically-update-notes-on-the-remote-server-whe)

> ⚠️ Git does not support automatically pushing notes via remote push refSpecs. This is a long-standing limitation of Git.

Although it might seem possible to configure Git to always push notes by adding a push refSpec, this configuration is ignored for notes:

```bash
# These do not work for notes:
git config remote.origin.push +refs/heads/*:refs/heads/*
git config --add remote.origin.push +refs/notes/*:refs/notes/*
```

If done this way then it changes the default git push behavior.

Even if specified, Git will not include notes refs in a regular `git push` operation. Notes must be pushed manually using an explicit command like:

```bash
git push origin refs/notes/enc-internal/commit/*
```

> 📌 Always push notes explicitly. If automation is required, consider scripting note pushes as part of your CI/CD or post-push hooks.
