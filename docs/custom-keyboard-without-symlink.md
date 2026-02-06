# 커스텀 키보드 빌드 (심볼릭 링크)

QMK는 **키보드 정의**를 `qmk_firmware` 쪽에서만 찾습니다. 이 레포의 `keyboards/` 에 넣은 커스텀 키보드를 쓰려면, 해당 경로를 로컬 qmk_firmware에 **심볼릭 링크**로 잡아 두면 됩니다.

- [External QMK Userspace (공식)](https://docs.qmk.fm/newbs_external_userspace)

---

## 설정 방법

**로컬 qmk_firmware**가 있는 경로(예: `~/qmk_firmware` 또는 `qmk config user.qmk_home`에 설정된 경로)의 `keyboards/` 아래에, 이 레포의 `keyboards/` 항목을 심볼릭 링크로 만듭니다.

`keyboards/` 아래에 있는 **디렉터리**마다 하나씩 링크하면 됩니다 (`.keep` 같은 파일은 제외).

```bash
# 예: 이 레포가 ~/repo/kbd/qmk_userspace 일 때
QMK_HOME=$(qmk config user.qmk_home | cut -d= -f2)
USPACE=/path/to/qmk_userspace

for d in "$USPACE/keyboards"/*/; do
  name=$(basename "$d")
  ln -sf "$(realpath "$d")" "$QMK_HOME/keyboards/$name"
done
```

수동으로 한 개만 걸 때 예:

```bash
ln -sf /Users/master/repo/kbd/qmk_userspace/keyboards/myfi /Users/master/qmk_firmware/keyboards/myfi
```

이후 `user.qmk_home`은 기존 그대로 두고 빌드하면 됩니다.

```bash
qmk compile -kb myfi/castor_kbd -km default
```

---

## 정리

| 항목 | 설명 |
|------|------|
| 키보드 소스 | 이 레포 `keyboards/<vendor>/` (예: `keyboards/myfi/`) |
| 링크 위치 | `qmk_firmware/keyboards/<vendor>` → 위 경로로 심볼릭 링크 |
| QMK 홈 | 기존 `user.qmk_home` 유지 (바꿀 필요 없음) |

새로 `keyboards/` 아래에 폴더를 추가했다면, 같은 방식으로 qmk_firmware 쪽에 심볼릭 링크만 한 번 더 걸어 주면 됩니다.
