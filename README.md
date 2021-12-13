### 1. Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.

**git show aefea**
```bash

Полный хеш коммита: aefead2207ef7e2aa5dc81a34aedf0cad4c32545

Комментарий:    Update CHANGELOG.md
```

### 2. Какому тегу соответствует коммит 85024d3?

**git show 85024d3**
```bash
Тег коммита: v0.12.23
```
### 3. Сколько родителей у коммита b8d720? Напишите их хеши.

**git show b8d720**

**git show b8d720^**

**git show b8d720^2**


```bash
У коммита 2 родителя.
Хеш певрого родителя: 56cd7859e05c36c06b56d013b55a252d0bb7e158
Хеш второго родителя: 9ea88f22fc6269854151c571162c5bcf958bee2b
```

### 4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

**git show v0.12.23..v0.12.24 --one-line**

```bash
33ff1c03b (tag: v0.12.24) v0.12.24
b14b74c49 [Website] vmc provider links
3f235065b Update CHANGELOG.md
6ae64e247 registry: Fix panic when server is unreachable
5c619ca1b website: Remove links to the getting started guide's old location
06275647e Update CHANGELOG.md
d5f9411f5 command: Fix bug when using terraform login on Windows
4b6d06cc5 Update CHANGELOG.md
dd01a3507 Update CHANGELOG.md
225466bc3 Cleanup after v0.12.23 release
```
### 5. Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).

**git log -S'func providerSource' --pretty=format:'%h %cd %an %ae %s'**

**git show 5af1e6234** 

**git show 8c928e835**
```bash
5af1e6234 Tue Apr 21 16:28:59 2020 -0700 Martin Atkins mart@degeneration.co.uk main: Honor explicit provider_installation CLI config when present
8c928e835 Mon Apr 6 09:24:23 2020 -0700 Martin Atkins mart@degeneration.co.uk main: Consult local directories as potential mirrors of providers
```

### 6. Найдите все коммиты в которых была изменена функция globalPluginDirs.

**git log -S'globalPluginDirs' --oneline**
```bash
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package
```
### 7. Кто автор функции synchronizedWriters?

**git log -S'synchronizedWriters' --oneline**
```bash
bdfea50cc remove unused
fd4f7eb0b remove prefixed io
5ac311e2a main: synchronize writes to VT100-faker on Windows
```

**git checkout 5ac311e2a91e381e2f52234668b49ba670aa0fe5**
```bash
Updating files: 100% (13492/13492), done.
Note: switching to '5ac311e2a91e381e2f52234668b49ba670aa0fe5'.
```
**git grep -c 'func synchronizedWriters'**
```bash
synchronized_writers.go:1
```
**git grep -n 'func synchronizedWriters'**
```bash
synchronized_writers.go:15:func synchronizedWriters(targets ...io.Writer) []io.Writer {
```
**git blame -C -L 15,30 synchronized_writers.go**
```bash
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 15) func synchronizedWriters(targets ...io.Writer) []io.Writer {
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 16)         mutex := &sync.Mutex{}
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 17)         ret := make([]io.Writer, len(targets))
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 18)         for i, target := range targets {
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 19)                 ret[i] = &synchronizedWriter{
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 20)                         Writer: target,
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 21)                         mutex:  mutex,
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 22)                 }
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 23)         }
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 24)         return ret
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 25) }
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 26)
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 27) func (w *synchronizedWriter) Write(p []byte) (int, error) {
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 28)         w.mutex.Lock()
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 29)         defer w.mutex.Unlock()
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 30)         return w.Writer.Write(p)
```
***Автор функции: Martin Atkins***