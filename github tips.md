* 仅显示真正改动：在URL结尾加上`?w=1`

```bash
# Moving a repository
git clone git@github.com:[owner]/[repo-name].git --bare
cd [repo-name]
git remote add enterprise git@[hostname]:[owner]/[repo-name].git
git push enterprise --mirror
```
