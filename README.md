# [미니프로젝트] 연차/당직 프로젝트 만들기 - 백엔드

## 개요

- 팀 프로젝트
- 2023.3.20 ~ 2023.4.6

## 기술
- Java
- Spring
- Spring Security
- H2

## 맡은 역할

- 회원 ERD 설계
- 회원 가입 구현
- 회원 가입 실패 시 메세지 우선 순위 맞게 프런트에 전달

### 회원 ERD 설계

![K-001](https://github.com/user-attachments/assets/b0311354-5444-4011-a034-05b007bad844)

### 회원 가입 스프링 시큐리티로 구현

![팀 프로젝트 연차 drawio](https://github.com/user-attachments/assets/def02f62-19c1-40f2-86dc-469763059d78)


### 회원 가입 실패 메세지 프런트 전달

```java
@Data
@AllArgsConstructor
@GroupSequence({
        MemberSaveForm.class,
        ValidationSteps.Step1.class,
        ValidationSteps.Step2.class,
        ValidationSteps.Step3.class,
        ValidationSteps.Step4.class,
        ValidationSteps.Step5.class,
        ValidationSteps.Step6.class,
        ValidationSteps.Step7.class,
        ValidationSteps.Step8.class,
        ValidationSteps.Step9.class,
        ValidationSteps.Step10.class,
})
public class MemberSaveForm {

    @NotEmpty(groups = ValidationSteps.Step1.class,
            message = "{MemberSaveForm.account.NotEmpty}")
    @Email(groups = ValidationSteps.Step2.class,
            message = "{MemberSaveForm.account.Email}")
    private String account;

    @NotEmpty(groups = ValidationSteps.Step3.class,
            message = "{MemberSaveForm.password.NotEmpty}")
    // 숫자와 문자 포함 형태의 6~12자리 이내의 암호 정규식
    @Pattern(regexp = "^[A-Za-z0-9]{6,12}$", groups = ValidationSteps.Step4.class,
            message = "{MemberSaveForm.password.Pattern}")
    private String password;

    @NotEmpty(groups = ValidationSteps.Step5.class,
            message = "{MemberSaveForm.name.NotEmpty}")
    @Length(min = 2, max = 5, groups = ValidationSteps.Step6.class,
            message = "{MemberSaveForm.name.Length}")
    private String name;

    @NotNull(groups = ValidationSteps.Step7.class,
            message = "{MemberSaveForm.birth.NotEmpty}")
    @Past(groups = ValidationSteps.Step8.class,
            message = "{MemberSaveForm.birth.Past}")
    private LocalDate birth;

    @NotNull(groups = ValidationSteps.Step9.class,
            message = "{MemberSaveForm.phoneNumber.NotEmpty}")
    @Pattern(regexp = "^01(?:0|1|[6-9])(?:\\d{3}|\\d{4})\\d{4}$", groups = ValidationSteps.Step10.class,
            message = "{MemberSaveForm.phoneNumber.Pattern}")
    private String phoneNumber;

}
```

- 회원 가입 실패 시 우선 순위에 맞게 실패 메세지 프런트로 전달

#### message.properties

```java
MemberSaveForm.account.NotEmpty=계정을 입력해주세요.
MemberSaveForm.account.Email=이메일 형식이 아닙니다.
MemberSaveForm.password.NotEmpty=비밀번호를 입력해주세요.
MemberSaveForm.password.Pattern=비밀번호는 숫자+문자를 조합한 6~12자리만 입력하실 수 있습니다.
MemberSaveForm.name.NotEmpty=이름을 입력해주세요.
MemberSaveForm.name.Length=이름의 길이는 2~5자리만 입력하실 수 있습니다.
MemberSaveForm.birth.NotEmpty=생일을 입력해주세요.
MemberSaveForm.birth.Past=올바른 생일을 입력해주세요.
MemberSaveForm.phoneNumber.NotEmpty=휴대전화 번호를 입력해주세요.
MemberSaveForm.phoneNumber.Patter=휴대전화 번호의 양식을 확인해주세요.
```

- 실패 메세지는 파일을 분리하여 관리

### 암호화

- 회원 정보의 성격에 맞게 단방향 / 양방향 암호화 선택
- [암호화](https://velog.io/@meteor_control0/%EC%95%94%ED%98%B8%ED%99%94-%EB%B3%B5%ED%98%B8%ED%99%94)
