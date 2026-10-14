# fabrik-test-delta

Secondary test bed for [Fabrik](https://github.com/handarbeit/fabrik), paired with [`fabrik-test-gamma`](https://github.com/handarbeit/fabrik-test-gamma).

This repository is not a real project. It exists purely as substrate for exercising Fabrik's multi-repo features (cross-repo sub-issue spawn, cross-repo dependency linkage, parallel multi-repo work, etc.) without polluting real project boards.

## Layout

- `pkg/greeting/` — exports `Greeting() string`, consumed by `fabrik-test-gamma`. Cross-repo Fabrik tests file issues against alpha that require a change here first.

## License

Apache 2.0 (matches Fabrik upstream).
