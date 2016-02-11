# Oh My ZSH `gi` git-ignore plugin
Here it is: [gitignore.plugin.zsh][1]

It's installed as usually in `~/.zshrc` file.

## What does it do?
It integrates with excellent [gitignore.io][2] and provides the following:
- available `.gitignore` templates auto completion
- the named `.gitignore` templates fetch

<!-- References -->
[1]: https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/gitignore/gitignore.plugin.zsh "gitignore zsh plugin"
[2]: https://www.gitignore.io/ "gitignore.io"

## Usage examples

**Let's see that's availablle**

```console
λ /test4/ master* gi
zsh: do you wish to see all 253 possibilities (64 lines)?
```

**Lest's search for `e`**

```console
λ /test4/ master* gi e
eagle             eiffelstudio      elixir            ensime            espresso
easybook          elasticbeanstalk  elm               episerver         expressionengine
eclipse           elisp             emacs             erlang            extjs
```

It's a lot. Let's be more specific.

**Let's fetch my favorite Intellij snippet**

```console
λ /test4/ master* gi intellij | tee -a .gitignore

# Created by https://www.gitignore.io/api/intellij

### Intellij ###
# Covers JetBrains IDEs: IntelliJ, RubyMine, PhpStorm, AppCode, PyCharm, CLion, Android Studio and Webstorm

*.iml

## Directory-based project format:
.idea/
# if you remove the above rule, at least ignore the following:

# User-specific stuff:
# .idea/workspace.xml
# .idea/tasks.xml
# .idea/dictionaries
# .idea/shelf

# Sensitive or high-churn files:
# .idea/dataSources.ids
# .idea/dataSources.xml
# .idea/sqlDataSources.xml
# .idea/dynamic.xml
# .idea/uiDesigner.xml

# Gradle:
# .idea/gradle.xml
# .idea/libraries

# Mongo Explorer plugin:
# .idea/mongoSettings.xml

## File-based project format:
*.ipr
*.iws

## Plugin-specific files:

# IntelliJ
/out/

# mpeltonen/sbt-idea plugin
.idea_modules/

# JIRA plugin
atlassian-ide-plugin.xml

# Crashlytics plugin (for Android Studio and IntelliJ)
com_crashlytics_export_strings.xml
crashlytics.properties
crashlytics-build.properties
fabric.properties


# mpeltonen/sbt-idea plugin
.idea_modules/

# JIRA plugin
atlassian-ide-plugin.xml

# Crashlytics plugin (for Android Studio and IntelliJ)
com_crashlytics_export_strings.xml
```

Here you go. Specific, carefully crafted and documented. Enjoy.

_Vuala_

