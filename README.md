# 1-DAY_Confirmation

# 1-DAY Vulnerability Research Repository

[![Repo Status](https://img.shields.io/badge/Status-Research-critical.svg)]()
[![Last updated](https://img.shields.io/badge/Last%20update-2025--10--25-lightgrey.svg)]()

> **Purpose:**  
> 본 저장소는 1-Day(단기) 취약점 리서치를 위해 운영되는 팀 저장소입니다. **취약점 분석 · PoC 재현 · 패치 검증**을 체계적으로 기록합니다.  
> 모든 실험은 **격리된 로컬 환경(랩)**에서 수행하며, 문서는 교육·연구 목적에 한정합니다.

---

## Quick Links

- [Team Notion Analysis DB](https://neighberhood-h.notion.site/2769e85bb8a9806aba9ffddf57bf34ed?v=2769e85bb8a980788c2f000cd39b859c#2769e85bb8a9805caface9da717905ab)   

- [CVE-2024-21821 — folder_sharing.lua 분석](./CVE-2024-21821/reports/analysis.md)  
- [CVE-2024-21773 — blocking.lua 분석](./CVE-2024-21773/reports/analysis.md)  
- [CVE-2024-3847 — firmware restore 분석](./CVE-2024-3847/reports/analysis.md)
- [CVE-2025-11005 — shttpd RCE 분석](./CVE-2025-11005/reports/analysis.md)

---

##  Repository Structure
```
1-DAY_Confirmation/   
├── CVE-YYYY-NNNNN/   
│   ├── reports/   
│   │   ├── LICENSE
│   │   └── analysis.md
│   └── PoC/
│       └── exploit.py
│
├── LICENSE
└── README.md   
 ```  
---

## Overview

| CVE | 취약 유형 | 영향 모듈 | 분석 범위 | PoC 여부 | 상태 |
|-----|------------:|------------|------------|:--------:|:----:|
| [CVE-2024-21821](./CVE-2024-21821/reports/analysis.md) | OS Command Injection (Unauthenticated) | `folder_sharing.lua` | 정적분석 · Diff · PoC · Emulation | ✅ | 완료 |
| [CVE-2024-21833](./CVE-2024-21833/reports/analysis.md) | URL Validation Bypass | `blocking.lua` | 정적분석 · PoC | ✅ | 완료 |
| [CVE-2024-3847](./CVE-2024-3847/reports/analysis.md) | OS Command Injection (Restore) | `firmware.lua` | 복호화 · Diff · 재현 | 🚧 | 진행중 |
| [CVE-2025-11005](./CVE-2025-11005/reports/analysis.md) | OS Command Injection (Unauthenticated) | `shttpd (setWiFiAclRules)` | 정적분석 · PoC | ✅ | 완료 |

---

## Analysis Format (Template)

각 `analysis.md`는 동일한 템플릿을 따릅니다.

1. **취약점 소개 (Introduction)** — 개요, 영향범위, 테스트 이미지  
2. **정적 분석 (Static Analysis)** — diff, 원인 함수, 패치 비교  
3. **에뮬레이션 / 재현 (Emulation)** — QEMU 환경, uhttpd 기동 등  
4. **Exploit / PoC** — 인증 흐름, 페이로드, 실행 흐름  
5. **재현 체크리스트** — 단계별 확인 항목  
6. **완화 및 패치 요약 (Mitigation)**  
7. **참고 자료 및 주의사항**

---

##  Environment Setup (Example)

> 기본 테스트 환경: Ubuntu 22.04 (or WSL), qemu-user-static, binwalk, python3

```bash
# 필수 도구 설치 Ubuntu
sudo apt update
sudo apt install -y binwalk qemu-user-static python3-pip build-essential
pip3 install pycryptodome

# 펌웨어 추출
binwalk -Me firmware.bin

# squashfs-root 
sudo cp /usr/bin/qemu-arm-static squashfs-root/usr/bin/
cd squashfs-root
sudo mount --bind /dev ./dev
sudo mount --bind /proc ./proc
sudo chroot . /bin/bash -c 'ubusd &'
sudo chroot . /usr/bin/qemu-arm-static /usr/sbin/uhttpd -f -h /www -x /cgi-bin -p 0.0.0.0:8080
```

---

##  Security & Ethics

본 저장소의 모든 PoC와 분석 자료는:
- 교육 및 연구 목적으로만 사용됩니다
- 격리된 로컬 환경에서만 테스트됩니다
- 무단 공격 행위를 절대 금지합니다
- 책임있는 보안 연구 윤리를 준수합니다

---

##  License
- © 2025 neighborhood-H.
- 모든 PoC는 교육 및 연구 목적에 한정되며, 실제 공격 목적으로 사용을 금합니다.
