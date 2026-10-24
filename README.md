# DarkRuins Bridge Cinematic

`LS_WeaponShot02_Bridge` 시퀀스를 중심으로 정리한 Unreal Engine 5.8 프로젝트입니다. 원래 Bridge 프로젝트는 보존하고, 별도 사본에서 시퀀스 주변의 환경과 캐릭터·차징·발사 효과에 필요한 의존성을 모았습니다. 기존 `/Game/...` 패키지 경로를 유지하므로 원본 프로젝트를 함께 복사할 필요가 없습니다.

## 프로젝트 열기

1. Unreal Engine **5.8**, Git, Git LFS를 준비합니다. 엔진 자체는 저장소에 포함하지 않습니다.
2. 저장소를 일반 Git clone으로 내려받고 아래 명령으로 실제 바이너리를 받습니다. GitHub ZIP만 내려받으면 LFS 포인터만 들어 있을 수 있습니다.

   ```powershell
   git lfs install
   git lfs pull
   git lfs fsck
   ```

3. `DarkRuins_Bridge_Cinematic.uproject`를 UE 5.8로 엽니다. 최초 셰이더·캐시 생성에는 시간이 필요합니다.
4. 맵 **`/Game/Main`**을 엽니다. 기본 시작 맵도 Main입니다.
5. 콘텐츠 브라우저의 **`/Game/BridgeTransfer_20261017/Sequences/LS_WeaponShot02_Bridge`**를 열고 Sequencer에서 카메라 컷을 따라 재생합니다.

프로젝트에 선언된 플러그인은 Water, WaterExtras, AlembicImporter, CableComponent, PythonScriptPlugin, EditorScriptingUtilities, SequencerScripting, MovieRenderPipeline입니다. 의존성 검사에서 사용한 엔진 측 경로는 Engine, Script, Niagara, ACLPlugin, InterchangeAssets입니다. 해당 구성요소를 포함한 UE 5.8 설치가 필요합니다.

## 폴더 구조

```text
DarkRuins_Bridge_Cinematic.uproject
Config/                 프로젝트 설정
Content/
  Main.umap             시퀀스용 맵
  BridgeTransfer_20261017/  주 시퀀스와 로컬 FX 재질·시스템
  __ExternalActors__/   World Partition 액터 패키지
  __ExternalObjects__/  World Partition 외부 오브젝트
  mY/, Paragon*/, MS/, CustomAssets/, ...  필요한 원래 경로의 자산
Docs/                   파일 manifest, 의존성, 검증 기록
```

`__ExternalActors__`와 `__ExternalObjects__`는 필수 맵 데이터입니다. 직접 삭제하거나 자산 폴더를 탐색기에서 옮기지 마세요. 경로 변경이 필요하면 Unreal 에디터에서 참조를 유지하며 처리해야 합니다.

`Saved`, `Intermediate`, `DerivedDataCache`, 렌더 결과, 로그는 Git에서 제외합니다. 에디터가 이 폴더를 다시 만들어도 프로젝트 자산이 늘어난 것은 아닙니다.

## 포함 범위와 용량

초기 배포 payload는 **1,804개 파일 / 7,567,000,541 bytes**입니다. 이 중 Unreal 자산 1,799개의 고유 LFS 내용은 **7,566,937,322 bytes**입니다. README·Docs·Git 메타데이터와 로컬에서 재생성되는 캐시는 별도입니다.

별도 작업 사본에서 시퀀스 밖 환경 액터와 불필요한 생성 HLOD를 정리한 뒤, 최종 맵·시퀀스의 의존성을 다시 계산했습니다. 최종 Main에는 액터 912개가 있습니다. 자유 탐험용 전체 환경이나 다른 카메라 구도를 모두 보존하는 배포본은 아닙니다. 원본 Bridge 프로젝트의 자산은 수정하지 않았습니다.

파일별 경로·크기·SHA-256은 [PAYLOAD_MANIFEST.csv](Docs/PAYLOAD_MANIFEST.csv), 패키지 의존성은 [DEPENDENCIES.json](Docs/DEPENDENCIES.json)에 있습니다.

## 검증 범위

최종 사본을 새 UE 프로세스의 NullRHI 환경에서 열어 액터 912개 자동 로드, 누락 패키지 0개, 제거한 패키지 참조 0개를 확인했습니다. 223개 프레임의 카메라 값과 시퀀스 키, Actor2 부모 관계, SHOOT2의 차징·발사 바인딩 및 Niagara 참조도 대조했습니다.

이는 **엔진 데이터·참조 검사 통과**입니다. 최종 슬림 사본의 GPU 화면·실시간 효과 외관·전체 영상 렌더는 아직 검증하지 않았습니다. NullRHI 평가 중 SHOOT2 관련 외부 액터 한 개가 메모리에서 dirty 상태가 되었지만 저장 호출 없이 종료했으며 배포 파일은 유지했습니다. 상세 근거와 Git 재현 검증은 [VALIDATION.json](Docs/VALIDATION.json), [PROJECT_NOTES.md](Docs/PROJECT_NOTES.md)에 기록합니다.

## 저장소 운영

이 프로젝트는 비공개 저장소 사용을 전제로 준비했습니다. Megascans·Paragon 등 외부 자산은 각 자산의 원래 이용 조건을 따릅니다. 저장소 접근 권한을 부여하기 전에 수신자의 사용 자격을 확인하고, 공개 저장소로 전환하거나 자산만 따로 재배포하지 마세요. 이 저장소는 외부 자산에 새 라이선스를 부여하지 않습니다.

LFS 바이너리는 수정 버전마다 전체 파일 용량이 추가될 수 있습니다. 최초 프로젝트가 10GB 미만이어도 계정 전체의 기존 사용량에 따라 무료 한도를 넘을 수 있으므로 업로드 전 실제 잔여량과 지출 차단 설정을 확인해야 합니다. 이 문서는 원격 업로드 완료나 무료 잔여량 확보를 뜻하지 않습니다.
