# Kind 기본 클러스터 가이드

로컬 Docker 환경에서 Kind로 최소 구성의 쿠버네티스 클러스터를 올리는 방법입니다. 함께 제공된 `kind-config.yaml` 설정을 사용합니다.

## 준비물
- Docker Desktop 또는 호환 컨테이너 런타임이 실행 중일 것
- `kind` CLI 설치: macOS/Homebrew 예) `brew install kind`
- `kubectl` 설치: macOS/Homebrew 예) `brew install kubernetes-cli`

## 클러스터 생성
1) 이 디렉터리로 이동
   ```bash
   cd Kubernetes/Kind
   ```
2) 기본 1 컨트롤 플레인 + 3 워커 노드 클러스터 생성
   ```bash
   kind create cluster --name private-cluster --config kind-config.yaml
   ```
3) kubeconfig 컨텍스트 확인 및 사용
   ```bash
   kubectl config use-context kind-private-cluster
   kubectl get nodes -o wide
   ```

## 정상 동작 확인
- API 서버 정보: `kubectl cluster-info`
- 노드 상태: `kubectl get nodes`
- 내부 DNS 확인: `kubectl run -it --rm dnsutils --image=registry.k8s.io/e2e-test-images/dnsutils:1.3 -- nslookup kubernetes.default`

## 클러스터 삭제
```bash
kind delete cluster --name private-cluster
```

## 구성 수정 포인트
- 노드 수, 역할, 포트 매핑 등은 `kind-config.yaml`에서 변경 후 `kind create cluster ...`를 다시 실행합니다.
- 추가 레지스트리 미러, Ingress, 스토리지 클래스 등 확장이 필요하면 Kind 공식 문서를 참고해 항목을 추가하세요.
