

## 常见前缀

### `feat`(Feature) 新功能

*   含义：表示新增功能（feature）的提交。
*   示例：`feat: add night mode to the application`
*   描述：用于描述新功能的添加，比如增加了一个新的用户界面特性或后端服务功能。


### `fix`(Fix) 修复 bug

*   含义：表示修复错误（bug fix）的提交。
*   示例：`fix: resolve null pointer exception in user login`
*   描述：用于修复代码中的错误或问题，确保程序的行为符合预期。


### `style`(Style) 代码风格变动，不影响代码逻辑

*   含义：表示代码样式修改的提交，不影响程序逻辑。
*   示例：`style: update code formatting according to ESLint rules`
*   描述：用于改进代码风格，例如调整缩进、空格、换行、规范化命名约定等，通常不会改变代码的功能。


### `revert`(Revert) 回滚

*   含义：用于提交回滚之前的提交。
*   示例：`revert: 回滚feat: 增加用户注册功能。`
*   描述：用于直接明确回滚的代码，包含了那些历史功能模块。


### `merge`(Merge) 合并分支或解决合并冲突。

*   含义：合并分支或解决合并冲突。
*	示例：`merge: 合并feature分支到主分支`
*   这个提交表示合并了一个分支到主分支或解决了合并冲突。


### `build`(Build) 构建系统或外部依赖项的变更

*   含义：用于提交影响构建系统的更改。
*   示例：`build: 升级webpack到版本5。`
*   说明：这个提交表示对构建系统或依赖项进行了更改，可能是更新了项目所依赖的软件包或构建工具。


### `refactor`(Refactor) 代码重构，即不改变代码功能的情况下优化代码结构。
    
*   含义：表示代码重构，即不改变代码功能的情况下优化代码结构。
*   示例：`refactor: simplify complex function logic`
* 	说明：这个提交表示对代码进行了重构，可能是优化代码结构或提高代码质量，但没有引入新功能或修复 bug。


### `docs`(Documentation) 文档变更

*   含义：表示文档更新，如 README 文件、API 文档等。
*   示例：`docs: update installation guide for v2.0`
* 	说明：这个提交表示对文档进行了更新，可能是添加或修改了API文档或其他项目文档。


### `test`(Test) 修改或添加测试

*   含义：表示测试相关的更改，如添加或修改测试用例。
*   示例：`test: add unit tests for new feature`
* 	说明：这个提交表示对测试方面进行了更改，可能是添加了新的集成测试用例或修改了现有测试。


### `chore`(Chore) 杂项（构建过程或辅助工具的变动）

*   含义：零碎、非功能性的任务的提交。比如，更新构建脚本，更新依赖库、项目工程方面的改动等。代码逻辑并未产生任何变化（没有 src 或者 test 的变动）。
*   示例：`chore: update package.json dependencies`
*   示例：`chore: 更新构建脚本`
*   说明：这个提交表示进行了一般性的维护工作，可能是更新了构建脚本或进行了其他与代码功能无关的任务。


### `perf`(Performance) 性能优化

*   含义：表示性能优化的提交。
*   示例：`perf: optimize database query performance`
* 	说明：这个提交表示对性能进行了优化，可能是提高了图像加载速度或其他性能方面的改进。


### `ci`(Continuous Integration) 持续继承的变更

*   含义：表示持续集成相关的更改，如 CI/CD 配置文件的更新。
*   示例：`ci: configure GitHub Actions for automated testing`
* 	说明：这个提交表示对持续集成或自动化流程进行了更改，例如更新部署脚本或配置。


### `release`(Release) 发布新版本或版本号更新
	
*   含义：发布新版本或版本号更新
*	示例：`release: 版本1.0.0发布`
*	这个提交表示项目发布了一个新的版本，可能包含了一系列的更改和修复。


## 其他常见前缀

*   hotfix: 表示热修复工作流类型,通常用于紧急修复生产环境中的问题。
*   init: 表示初始化工作流类型,用于项目初始化或初始提交。
*   workflow: 表示提交的工作流或过程类型,以便更好地理解提交的上下文和目的。
*   wip: wip 是Work In Progress的缩写,即工作正在进行中。表示提交仍然处于开发或尚未完成的状态。