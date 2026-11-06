# Commons Android #7120 validation evidence

These recordings compare the same public Church search, media detail, Back, recent search, Categories and Items flows in BetaDebug on a Samsung SM-S921E, Android 16 / API 36.

- [Before: original e216d7c](before.mp4)
- [After: refactor f086928](after.mp4)
- [Before: Detekt output](before-detekt.txt)
- [After: Detekt output](after-detekt.txt)

This is a static-analysis refactor, so the visible behavior is intentionally preserved. The beta data includes an unavailable thumbnail and no matching Church categories in both builds. Debug LeakCanary notices also appear in both recordings.

Removing only the five requested baseline exceptions from the original source produces exactly five TooManyFunctions findings. The refactored source passes Detekt with those exceptions removed:

| Class | Original functions | Refactored functions |
| --- | ---: | ---: |
| BasePagingFragment | 13 | 7 |
| ExploreListRootFragment | 12 | 7 |
| ExploreMapRootFragment | 15 | 8 |
| ExploreMapPresenter | 14 | 10 |
| SearchActivity | 14 | 10 |

The pinned Detekt version flags counts at or above 11. Restoring each original extraction group reproduces its expected findings (paging: 1; roots: 2; markers: 1; search: 1); restoring the refactored source passes again.

Required Linux checks: ktlintCheck, detekt and testBetaDebugUnitTest passed. Unit tests: 1,830 passed, 41 existing skips, zero failures/errors. Nineteen regression tests were added.

Additional Linux checks passed: lintBetaDebug, assembleBetaDebug and assembleProdDebug. The contributor confirmed two own-photo beta uploads completed successfully.

This evidence branch is separate from the code pull request; its video files do not enter the upstream code diff.
