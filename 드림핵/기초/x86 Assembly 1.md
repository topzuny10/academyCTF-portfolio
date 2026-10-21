## 어셈블리어 기본 규칙
**Intel문법**과 **AT&T문법**으로 나뉘며 Intel 문법은 NAS(Netwide Assembler)에서 사용하며, AT&T 문법은 GAS(Gnu Assembler)에서 사용된다.

## x86-64 어셈블리어
기본 구조는 **명령어(Operation code, Opcode)** 와 **피연산자(Operand)** 로 구성된다. *피연산자는 해당 연산의 입력 값(레지스터, 상수, 메모리 주소 등)* 이 된다. 이때 피연산자의 개수는 0개 혹은 그 이상이 될 수 있다.

	예시: (opcode) (operand1) (operand2)
		 대입해라      op1에       op2를
		  
	mov eax, 3
	eax에 3을 대입해라

## 명령어
	데이터 이동: mov, lea
	산술 연산: inc, dec, add, sub
	논리 연산: and, or, xor, not
	비교: cmp, test
	분기: jmp, je, jg
	스택: push, pop
	프로시져: call, ret, leave
	시스템 콜: syscall

## 피연산자
피연산자는 총 3가지 종류로 **상수, 레지스터, 메모리**가 올 수 있다.
##### 피연산자 표기법
	r8: 8비트 범용 레지스터(ah, al, dh, dl...)
	r16: 16비트 범용 레지스터(ax, bx, cx...)
	r32: 32비트 범용 레지스터(eax, ebx, ecx...)
	r64: 64비트 범용 레지스터(rax, rbx, rcx...)
	imm8: 8비트 부호있는 정수
	imm16: 16비트 부호있는 정수
	imm32: 32비트 부호있는 정수
	imm64: 64비트 부호있는 정수
	r/m8: 8비트 범용 레지스터 혹은 메모리
	r/m16: 16비트 범용 레지스터 혹은 메모리
	r/m32: 32비트 범용 레지스터 혹은 메모리
	r/m64: 64비트 범용 레지스터 혹은 메모리

## 산술 연산과 논리 연산
	add: 덧셈
	sub: 뺄셈
	mul, imul: 곱셈
	div, idiv: 나눗셈
x86-64에서는 **부호 없는 연산(mul, div)과** **부호 있는 연산(imul, idiv)로** 구분함
예시: add <레지스터>, <값>
#### 논리 연산
	and: 두 값이 모두 1일 때만 결과가 1이 된다. 주로 비트 마스킹(Bit Masking)에 사용된다.
	or: 하나 이상의 비트가 1이면 결과가 1이 된다. 특정 비트를 설정(Set)하는 데 사용된다.
	xor: 두 개의 비트가 다를 때만 결과가 1이 된다. 주로 비트 토글(Toggling) 및 암호화 등에 사용된다.
	not: 비트를 반전(Complement)한다.
예시:
AND (destination), (source) <source>
NOT (destination)

### inc, dec
	inc - 1증가
	dec - 1감소

inc <레지스터/메모리>

## 데이터 저장과 이동
	mov(move): 데이터를 복사/이동
	mov <desination>, <source>
	lea: 메모리에 실제 접근을 하지 않고 유효 주소를 저장하기 위해 사용

## 비교 연산과 분기문
	cmp: 내부적으로 두 피연산자를 빼는 방식으로 플래그를 갱신
	cmp <destination>, <source>
	test: 내부적으로 <destination>과 <source>를 AND연산 후 결과는 버리고 플래스 레지스터만 갱신함, 보통 특정 비트가 0인지 아닌자 확인하거나, 레지스터가 0인지 검사할 때 씀.

## 분기
분기 명령어는 프로그램의 실행 흐름을 바꾸는 명령어로, 해당 명령어를 사용하면 **명령어 포인터(Instruction Pointer)** 가 변경된다.

	jmp: 특정 주소로 rip를 바꿈, 아무 조건 필요 없이 무조건 지정된 주소로 점프함
	je(jump if Equal), jz(jump if Zero): Zero Flag(ZF)가 1이면 점프함. 두 피연산자의 값이 같을 때 특정 동작을 수행하고 싶을 때 씀
	jg(jump if Greater), jge(jump if greater or equal): 두 피연산자 중 어떤 값이 더 클 때 특정 동작을 수행하고 싶을 때 사용하며, *jg는 두 피연산자 중 왼쪽 피연산자가 더 클 때 점프*, *jge는 두 피연산자 중 왼쪽 피연산자가 더 크거나 같을 때 점프함*.
	jl(Jump if Less), jle(Jump if Less or Equal): 두 피연산자 중 어떤 값이 더 작을 때 특정 동작을 수행하고 싶을 때 씀, jg / jge 명령어와 부호가 반대로 동작한다.
	jne(Jump if Not Equal), jnz(Jump if Not Zero): 두 피연산자가 다를 때 특정 동작을 수행하고 싶을 때 사용, jne와 jnz는 이름만 다를 뿐 동일한 기능을 수행하며 지정된 주소로 점프한다.