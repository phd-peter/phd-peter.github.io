# Chirpy Starter

[![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy)][gem]&nbsp;
[![GitHub license](https://img.shields.io/github/license/cotes2020/chirpy-starter.svg?color=blue)][mit]

When installing the [**Chirpy**][chirpy] theme through [RubyGems.org][gem], Jekyll can only read files in the folders
`_data`, `_layouts`, `_includes`, `_sass` and `assets`, as well as a small part of options of the `_config.yml` file
from the theme's gem. If you have ever installed this theme gem, you can use the command
`bundle info --path jekyll-theme-chirpy` to locate these files.

The Jekyll team claims that this is to leave the ball in the user’s court, but this also results in users not being
able to enjoy the out-of-the-box experience when using feature-rich themes.

To fully use all the features of **Chirpy**, you need to copy the other critical files from the theme's gem to your
Jekyll site. The following is a list of targets:

```shell
.
├── _config.yml
├── _plugins
├── _tabs
└── index.html
```

To save you time, and also in case you lose some files while copying, we extract those files/configurations of the
latest version of the **Chirpy** theme and the [CD][CD] workflow to here, so that you can start writing in minutes.

## Usage

Check out the [theme's docs](https://github.com/cotes2020/jekyll-theme-chirpy/wiki).
윈도우에서 "bundle exec jekyll s" 로 가상서버 구현가능

## Mac jekyll 환경설치
`bundle exec jekyll s`(또는 `bundle exec jekyll serve`) 명령은 운영체제와 무관하게 Jekyll 사이트를 로컬에서 실행하는 표준 방법입니다.
Windows에서만 동작하는 것이 아니라 macOS·Linux에서도 동일하게 사용할 수 있어요.

1. Ruby & Bundler 설치  
   • macOS 14(Sonoma) 이상에는 시스템에 기본 Ruby(3.0 계열)가 있지만, 별도 버전 관리 툴(rbenv, asdf 등)을 쓰면 충돌을 피하고 향후 업그레이드도 쉽습니다.  
   • Homebrew로 예시:  
     ```bash
     brew install ruby        # (선택) 최신 루비
     echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
     source ~/.zshrc
     gem install bundler
     ```

2. 프로젝트 디렉터리로 이동 후 의존성 설치  
   ```bash
   cd /Users/peter/Projects/phd-peter.github.io
   bundle install    # Gemfile에 정의된 테마·플러그인 설치
   ```

3. 개발용 서버 실행  
   ```bash
   bundle exec jekyll s     # 또는 bundle exec jekyll serve
   ```
   기본적으로 http://localhost:4000 에 사이트가 뜹니다. `--livereload` 옵션을 주면 파일 변경 시 자동 새로고침도 됩니다.


## Contributing

This repository is automatically updated with new releases from the theme repository. If you encounter any issues or want to contribute to its improvement, please visit the [theme repository][chirpy] to provide feedback.

## License

This work is published under [MIT][mit] License.

[gem]: https://rubygems.org/gems/jekyll-theme-chirpy
[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy/
[CD]: https://en.wikipedia.org/wiki/Continuous_deployment
[mit]: https://github.com/cotes2020/chirpy-starter/blob/master/LICENSE
