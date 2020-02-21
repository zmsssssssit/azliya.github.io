# Azliya github.io

> I'm using a [Ported Theme](https://github.com/Kaijun/hexo-theme-huxblog) of [Hux Blog](https://github.com/Huxpro/huxpro.github.io), Thank [Huxpro](https://github.com/Huxpro) for designing such a flawless theme.

##### 1.Init

```
npm install
```

##### 2.Modify

Modify `_config.yml` file with your own info.
Especially the section:

```
deploy:
  type: git
  repo: https://github.com/xxx
  branch: master
```

Replace with your own repo!

##### 3.Writting/Serve/Deploy

```
hexo new post IMAPOST
hexo serve // run hexo in local environment
npm run deploy // hexo will push the static files automatically into the specific branch(master) of your repo!
```

##### 4.Enjoy!

Please [**Star**](https://github.com/kaijun/hexo-theme-huxblog/stargazers) this theme Project if you like it! [**Following**](https://github.com/Kaijun) would also be appreciated!
