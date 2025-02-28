
> [!help] SOLID
> Это принципы, которые помогают писать понятный и гибкий код.
>
>>[!info] Принципы SOLID в контексте React
>>1. **SRP (Single Responsibility Principle)**
>>      - Компонент должен отвечать только за одну задачу. Не создавайте компоненты, которые делают слишком много. Например, компонент UserProfile не должен заниматься и отображением данных, и их загрузкой, и обработкой форм.
>>
>>2. **OCP (Open-Closed Principle)**
>>      - Компоненты должны быть открыты для расширения, но закрыты для модификации. Используйте props и композицию для добавления функциональности. Например, вместо изменения существующего Button, создайте PrimaryButton, расширяющий его возможности.
>>
>>3. **LSP (Liskov Substitution Principle)**
>>      - Дочерние компоненты должны корректно заменять родительские без нарушения работы приложения. Если вы заменяете базовый компонент на его наследника, приложение должно продолжать работать правильно.
>>
>>4. **ISP (Interface Segregation Principle)**
>>      - Не заставляйте компоненты принимать props, которые им не нужны. Разделяйте интерфейсы на более мелкие и специфичные. Например, вместо одного компонента Form с множеством props, создайте более специализированные компоненты SearchForm и RegistrationForm.
>>
>>5. **DIP (Dependency Inversion Principle)**
>>      - Высокоуровневые компоненты не должны зависеть от низкоуровневых деталей. Используйте контекст, хуки или props для передачи зависимостей. Например, компонент не должен напрямую использовать конкретный API, а получать данные через props или хуки.
>
>>[!example]  **Single Responsibility** (Единая ответственность)
>>**Суть**: Каждый компонент должен решать одну задачу.
>>**Решение**: Разделить на два компонента.
>>```jsx 
>>// Плохо: компонент и грузит данные, и отображает их
>>function UserProfile() {
>>  const [user, setUser] = useState(null);
>>
>>  useEffect(() => {
>>    fetchUser().then(data => setUser(data));
>>  }, []);
>>
>>  return (
>>    <div>
>>      {user ? <div>{user.name}</div> : <div>Loading...</div>}
>>    </div>
>>  );
>>}
>>
>>
>>// Хорошо: компонент только для загрузки данных
>>function UserLoader({ children }) {
>>  const [user, setUser] = useState(null);
>>
>>  useEffect(() => {
>>    fetchUser().then(data => setUser(data));
>>  }, []);
>>
>>  return children(user);
>>}
>>
>>// Хорошо: компонент только для отображения
>>function UserDisplay({ user }) {
>>  return <div>{user ? user.name : 'Loading...'}</div>;
>>}
>>
>>// Использование:
>><UserLoader>
>>  {(user) => <UserDisplay user={user} />}
>></UserLoader>
>>```
>
>>[!example]  **Open/Closed** (Открытость/закрытость)
>>**Суть**: Компоненты должны расширяться без изменения их исходного кода.
>>**Пример**: Базовая кнопка, которую можно кастомизировать.
>>```jsx 
>>// Базовая кнопка
>>function Button({ onClick, children, style }) {
>>  return (
>>    <button onClick={onClick} style={{ padding: '8px', ...style }}>
>>      {children}
>>    </button>
>>  );
>>}
>>
>>// Расширение: кнопка с иконкой
>>function IconButton({ icon, ...props }) {
>>  return (
>>    <Button {...props} style={{ ...props.style, paddingLeft: '32px' >>}}>
>>     <span className="icon">{icon}</span>
>>      {props.children}
>>    </Button>
>>  );
>>}
>>```
>> Теперь `IconButton` добавляет иконку, но не меняет код `Button`.
>
>>[!example]  **Liskov Substitution** (Подстановка Лисков)
>>**Суть**: Дочерние компоненты должны работать вместо родительских.
>>**Пример**: Компонент `Modal` и его улучшенная версия.
>>```jsx
>>// Базовая модалка
>>function Modal({ isOpen, onClose, children }) {
>>  return isOpen ? (
>>    <div className="modal">
>>      <button onClick={onClose}>×</button>
>>      {children}
 >>   </div>
>>  ) : null;
>>}
>>
>>// Улучшенная модалка с заголовком
>>function AdvancedModal({ title, ...props }) {
>>  return (
>>    <Modal {...props}>
>>      <h2>{title}</h2>
>>      {props.children}
>>    </Modal>
>>  );
>>}
>>```
>>`AdvancedModal` можно использовать везде, где ожидается `Modal`.
>
>>[!example]  **Interface Segregation** (Разделение интерфейсов)
>>**Суть**: Не заставлять компонент принимать ненужные пропсы.
>>**Пример**: Компонент аватара, которому не нужен весь объект пользователя.
>>```jsx 
>>// Плохо: компонент принимает весь объект user
>>function Avatar({ user }) {
>>  return <img src={user.avatarUrl} alt={user.fullName} />;
>>}
>>
>>// Хорошо: компонент принимает только нужные данные
>>function Avatar({ avatarUrl, altText }) {
>>  return <img src={avatarUrl} alt={altText} />;
>>}
>>```
>>Теперь `Avatar` не зависит от структуры объекта `user`.
>
>>[!example]  **Dependency Inversion** (Инверсия зависимостей)
>>**Суть**: Компоненты не должны зависеть от конкретных реализаций.
>>**Пример**: Передача данных через пропсы, а не их загрузка внутри.
>>```jsx 
>>// Плохо: компонент сам загружает данные
>>function UserList() {
>>  const [users, setUsers] = useState([]);
>>
>>  useEffect(() => {
>>    fetchUsers().then(data => setUsers(data));
>>  }, []);
>>
>>  return users.map(user => <div key={user.id}>{user.name}</div>);
>>}
>>
>>// Хорошо: данные передаются через пропсы
>>function UserList({ users }) {
>>  return users.map(user => <div key={user.id}>{user.name}</div>);
>>}
>>
>>// Где-то выше:
>><UserList users={fetchedUsers} />
>>```
>>Теперь `UserList` можно переиспользовать с любыми данными.
>
>> [!summary]
>>1. **Single Responsibility (Единая ответственность)**
>>      - Суть: Каждый компонент должен решать одну задачу.
>>2. **Open/Closed (Открытость/закрытость)**
>>      - Суть: Компоненты должны расширяться без изменения их исходного кода.
>>3. **Liskov Substitution (Подстановка Лисков)**
>>      - Суть: Дочерние компоненты должны работать вместо родительских.
>>4. **Interface Segregation (Разделение интерфейсов)**
>>      - Суть: Не заставлять компонент принимать ненужные пропсы.
>>5. **Dependency Inversion (Инверсия зависимостей)**
>>      - Суть: Компоненты не должны зависеть от конкретных реализаций. ***(Передача данных через пропсы, а не их загрузка внутри)***
>