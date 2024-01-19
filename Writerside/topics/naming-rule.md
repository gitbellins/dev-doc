# 명명규칙

계층형 아키텍쳐의 각 레이어 별

## package
`bellins.` + 시스템명
```Java
package bellins.demo;

public class DemoApplication {

    public static void main(String[] args) {
    }
}

```

## controller
도메인 별 CRUD 에 따라 구분하여 생성
단순 CRUD 로 표현하기 애매한 업무나 큰 로직이 있으면 별도로 생성

```Java
UserCreateController
UserReadController
UserUpdateController
UserDeleteController
```

## service
도메인 별 CRUD 에 따라 구분하여 생성
```Java
UserCreateService
UserReadService
UserUpdateService
UserDeleteService
```

## repository
도메인별 패키지 아래 repo 밑으로 생성
JPA Repository 인경우 CRUD 구분 없이
Mybatis Mapper 인 경우 CRUD 구분

```Java
UserRepository

UserCreateMapper
UserReadMapper
UserUpdateMapper
UserDeleteMapper
```

## DTO {id="dto"}

### Request & Response DTO
사용자의 API 요청 Request 와 결과 Response.   
기본적으로 Controller 로 들어오는 건 Request, Controller 가 반환하는 건 Response.   
Controller 에서 가공이 없을 경우 Service 에서 Controller 로 반환하는 DTO 도 Response 라 칭할 수 있음   
기본적으로 도메인 + (액션 | 구분자) + 접미어


#### 페이징
접미어로 `PageRequest`, `PageResponse` 사용   
페이징이 있는 리스트

```Java
@GetMapping("/user")
public UserSearchPageResponse search(UserSearchPageRequest req) {
    return userService.search(req);
}
```

#### 리스트
접미어로 `ListRequest`, `ListResponse` 사용   
페이징이 없는 리스트

```Java
@GetMapping("/user")
public UserSearchListResponse search(UserSearchListRequest req) {
    return userService.search(req);
}
```

#### 상세
접미어로 `ReadReqeust`, `ReadResponse` 사용

```Java
@GetMapping("/user/{id}")
public UserReadResponse get(@PathVariable long id) {
    return userService.get(id);
}

@GetMapping("/user/{id}")
public UserReadResponse get(@PathVariable long id) {
    UserReadRequest req = New UserReadRequest(id);
    req.set(...)
    
    return userService.get(req);
}

@GetMapping("/user")
public UserReadResponse get(@ModelAttribute UserReadRequest req) {
    User user = userService.get(req)
    
    return new UserReadResponse(user);
}
```
{collapsible="true" include-lines="3"}

#### 등록
접미어로 `CreateRequest`, `CreateResponse` 사용

```Java
@PostMapping("/user")
public UserCreateResponse create(UserCreateRequest req) {
    return userService.create(req);
}
```

#### 수정
접미어로 `UpdateRequest`, `UpdateResponse` 사용

```Java
@PutMapping("/user")
public UserUpdateResponse update(UserUpdateRequest req) {
    return userService.update(req);
}

@PutMapping("/user/{id}")
public UserUpdateResponse update(@PathVariable long id, UserUpdateRequest req) {
    req.setId(id);
    
    return userService.update(req);
}
```

#### 삭제
접미어로 `DeleteRequest`, `DeleteResponse` 사용

```Java

@DeleteMapping("/user/{id}")
public UserDeleteResponse delete(@PathVariable long id) {
    UserDeleteResponse req = New UserDeleteResponse(id);
    
    return userService.update(req);
}

@DeleteMapping("/user/{id}")
public UserDeleteResponse delete(@PathVariable long id, UserDeleteRequest req) {
    req.setId(id);
    
    return userService.update(req);
}

@DeleteMapping("/user")
public UserDeleteResponse delete(UserDeleteRequest req) {
    return userService.save(req);
}
```

#### 등록 & 수정
접미어로 `SaveRequest`, `SaveResponse` 사용

```Java
@PostMapping("/user")
public UserSaveResponse update(UserSaveRequest req) {
    return userService.save(req);
}
```

### 일반 DTO {id="general-DTO"}
Controller 이외에서 사용하는 DTO  
메소드 호출 인자나 결과값으로 사용  
기본적으로 도메인 + (액션 | 구분자) + 접미어(DTO)

```Java
UserReadResponse read(UserRequest req) {
    UserReadDTO userDTO = userRepository.findById(req.getId());
    
    return new UserReadResponse(userDTO);
}
```

## entity
도메인명(테이블명) + 접미어(Entity)

```Java
@Table(name="user")
@Entity
public class UserEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    ...
}
```

## View Template
자바 클래스와 같이 대문자로 시작하는 카멜케이스를 사용한다.
```Java
Main.jsp
Main.mustache
Main.html
Main.jte
```
