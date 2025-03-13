# TypeScript & React 4주 특강 커리큘럼

## 📋 과정 개요

| 항목      | 내용                                |
| --------- | ----------------------------------- |
| 교육 기간 | 4주 (주 1회, 3시간)                 |
| 대상      | TypeScript와 React 입문자           |
| 선수 지식 | JavaScript 기초 문법, HTML/CSS 기초 |
| 개발 환경 | Node.js, VS Code, Git               |

## 🎯 학습 목표

- TypeScript의 기본 문법과 타입 시스템 이해
- React 컴포넌트 기반 개발 방법론 습득
- TypeScript와 React를 결합한 실전 개발 능력 향상

## 📚 주차별 커리큘럼

### 1주차: TypeScript 기초 (1/2)

#### 1교시: TypeScript 소개

```typescript
// TypeScript 기본 설정
{
"compilerOptions": {
  "target": "es5",
  "module": "commonjs",
  "strict": true,
  "esModuleInterop": true,
  "skipLibCheck": true,
  "forceConsistentCasingInFileNames": true
}
}
```

#### 2교시: 기본 타입

```typescript
// 기본 타입 예제
let name: string = "John";
let age: number = 30;
let isStudent: boolean = true;

// 배열
let numbers: number[] = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];

// 튜플
let tuple: [string, number] = ["hello", 10];

// enum
enum Color {
  Red,
  Green,
  Blue,
}
let color: Color = Color.Red;
```

#### 3교시: 실습

- TypeScript Playground 활용
- 기본 타입을 활용한 간단한 프로그램 작성

### 2주차: TypeScript 기초 (2/2)

#### 1교시: 인터페이스와 타입

```typescript
// 인터페이스 정의
interface User {
  id: number;
  name: string;
  email?: string;
  readonly birthDate: Date;
}

// 타입 별칭
type Point = {
  x: number;
  y: number;
};

// 인터페이스 확장
interface Employee extends User {
  department: string;
  salary: number;
}
```

#### 2교시: 함수와 제네릭

```typescript
// 함수 타입
function add(x: number, y: number): number {
  return x + y;
}

// 제네릭 함수
function getFirstElement<T>(arr: T[]): T | undefined {
  return arr[0];
}

// 제네릭 인터페이스
interface Response<T> {
  data: T;
  status: number;
  message: string;
}
```

### 3주차: React와 TypeScript 기초 & Todo List (1/2)

### 1교시: React + TypeScript 기초

#### 이론 (30분)

- Create React App with TypeScript 설정
- TSX 문법과 타입 지정
- React 컴포넌트 타입 정의

```tsx
// 컴포넌트 props 타입 정의
interface TodoItemProps {
  id: number;
  text: string;
  completed: boolean;
  onToggle: (id: number) => void;
  onDelete: (id: number) => void;
}

// 함수형 컴포넌트 정의
const TodoItem: React.FC<TodoItemProps> = ({
  id,
  text,
  completed,
  onToggle,
  onDelete,
}) => {
  return (
    <div className={`todo-item ${completed ? "completed" : ""}`}>
      <input
        type="checkbox"
        checked={completed}
        onChange={() => onToggle(id)}
      />
      <span>{text}</span>
      <button onClick={() => onDelete(id)}>삭제</button>
    </div>
  );
};
```

#### 실습 (30분)

- CRA로 프로젝트 생성
- 기본 컴포넌트 구조 설계
- Props 타입 정의 실습

### 2교시: React Hooks with TypeScript

#### 이론 (30분)

- useState와 타입 추론
- useEffect 활용
- 커스텀 훅 만들기

```tsx
// Todo 타입 정의
interface Todo {
  id: number;
  text: string;
  completed: boolean;
}

// 커스텀 훅 예제
function useTodos(initialTodos: Todo[] = []) {
  const [todos, setTodos] = useState<Todo[]>(initialTodos);

  const addTodo = useCallback((text: string) => {
    setTodos((prev) => [...prev, { id: Date.now(), text, completed: false }]);
  }, []);

  const toggleTodo = useCallback((id: number) => {
    setTodos((prev) =>
      prev.map((todo) =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  }, []);

  return { todos, addTodo, toggleTodo };
}
```

#### 실습 (30분)

- Todo 상태 관리 구현
- CRUD 기능 구현
- 커스텀 훅 작성

### 3교시: Todo List 기본 기능 구현

#### 실습 (60분)

- Todo 입력 폼 구현
- Todo 목록 표시
- Todo 완료/삭제 기능

```tsx
// TodoList 컴포넌트
const TodoList: React.FC = () => {
  const [newTodo, setNewTodo] = useState<string>("");
  const { todos, addTodo, toggleTodo } = useTodos();

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (newTodo.trim()) {
      addTodo(newTodo.trim());
      setNewTodo("");
    }
  };

  return (
    <div className="todo-list">
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="할 일을 입력하세요"
        />
        <button type="submit">추가</button>
      </form>
      {/* Todo 아이템 렌더링 */}
    </div>
  );
};
```

## 4주차: Todo List 완성 & API 연동 (2/2)

### 1교시: API 연동과 비동기 처리

#### 이론 (30분)

- Axios와 TypeScript
- API 응답 타입 정의
- 에러 처리

```typescript
// API 타입 정의
interface TodoResponse {
  id: number;
  text: string;
  completed: boolean;
  createdAt: string;
}

// API 클라이언트 설정
const api = axios.create({
  baseURL: "https://api.example.com/todos",
});

// API 함수 정의
async function fetchTodos(): Promise<TodoResponse[]> {
  const response = await api.get<TodoResponse[]>("/");
  return response.data;
}
```

#### 실습 (30분)

- API 클라이언트 구현
- 데이터 fetching 로직 작성

### 2교시: 상태 관리와 로딩/에러 처리

#### 이론 (30분)

- 로딩 상태 관리
- 에러 바운더리
- React Query 소개와 활용

```tsx
// React Query를 활용한 Todo 관리
function useTodoQuery() {
  const queryClient = useQueryClient();

  const todosQuery = useQuery<TodoResponse[], Error>("todos", fetchTodos);

  const addTodoMutation = useMutation(
    (newTodo: { text: string }) => api.post("/todos", newTodo),
    {
      onSuccess: () => {
        queryClient.invalidateQueries("todos");
      },
    }
  );

  return {
    todos: todosQuery.data,
    isLoading: todosQuery.isLoading,
    error: todosQuery.error,
    addTodo: addTodoMutation.mutate,
  };
}
```

#### 실습 (30분)

- 로딩 컴포넌트 구현
- 에러 처리 구현
- React Query 적용

### 3교시: 마무리 및 배포

#### 실습 (60분)

- 스타일링 완성
- 성능 최적화
- 배포 준비

```tsx
// 최적화된 Todo 컴포넌트
const TodoItem = memo<TodoItemProps>(({ id, text, completed, onToggle }) => {
  return (
    <div className="todo-item">
      <input
        type="checkbox"
        checked={completed}
        onChange={() => onToggle(id)}
      />
      <span className={completed ? "completed" : ""}>{text}</span>
    </div>
  );
});

// 최종 배포 체크리스트
interface DeploymentChecklist {
  optimization: boolean; // 성능 최적화
  errorHandling: boolean; // 에러 처리
  accessibility: boolean; // 접근성
  responsive: boolean; // 반응형 디자인
  testing: boolean; // 테스트 코드
}
```

## 📝 최종 프로젝트 요구사항

### 필수 기능

1. Todo CRUD 기능
2. API 연동
3. 로딩/에러 상태 처리
4. 반응형 디자인

### 선택 기능

1. 필터링 (전체/완료/미완료)
2. 로컬 스토리지 연동
3. 드래그 앤 드롭
4. 다크 모드

### 평가 기준

| 평가항목  | 배점 | 세부내용                         |
| --------- | ---- | -------------------------------- |
| 기능 구현 | 40%  | 필수 기능 완성도                 |
| 코드 품질 | 30%  | TypeScript 활용도, 컴포넌트 설계 |
| UI/UX     | 20%  | 사용자 경험, 반응형 디자인       |
| 최적화    | 10%  | 성능, 재사용성                   |

## 🔍 참고 자료

- [React Query 공식 문서](https://tanstack.com/query/latest)
- [Axios 타입스크립트 가이드](https://axios-http.com/docs/typescript)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
