# 1
> [!help] 

# 1
> [!help] 

# 1
> [!help] 

# React-portals
> [!help] 
>> [!quote] **React Порталы `(React.createPortal)`** 
 Порталы позволяют рендерить дочерние элементы в DOM-узел, находящийся вне иерархии родительского компонента
>
> ###### **Базовая структура HTML** - Создание целевого контейнера
>
>```html
><div id="root">
>    <!-- Основное приложение React -->
></div>
><div id="portal-root">
>    <!-- Сюда будет рендериться портал -->
></div>
>```
>
> ######  **Создание простого портала** - Базовый пример модального окна
>
>```jsx
>import ReactDOM from 'react-dom';
>
>const Modal = ({ children }) => {
>    const portalElement = document.getElementById('portal-root');
>    
>    return ReactDOM.createPortal(
>        children,
>        portalElement
>    );
>};
>```
>
> ###### **Пример использования модального окна**
>
>```jsx
>const App = () => {
>    const [isOpen, setIsOpen] = useState(false);
>
>    return (
>        <div>
>            <button onClick={() => setIsOpen(true)}>
>                Открыть модальное окно
>            </button>
>
>            {isOpen && (
>                <Modal>
>                    <div className="modal">
>                        <h2>Модальное окно</h2>
>                        <button onClick={() => setIsOpen(false)}>
>                            Закрыть
>                        </button>
>                    </div>
>                </Modal>
>            )}
>        </div>
>    );
>};
>```
>
> **Стили для модального окна**
>
>```css
>.modal {
>    position: fixed;
>    top: 50%;
>    left: 50%;
>    transform: translate(-50%, -50%);
>    background: white;
>    padding: 20px;
>    border-radius: 8px;
>    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
>    z-index: 1000;
>}
>```
>
> ###### **Портал с оверлеем** - Более сложный пример
>
>```jsx
>const ModalWithOverlay = ({ children, onClose }) => {
>    const portalElement = document.getElementById('portal-root');
>    
>    return ReactDOM.createPortal(
>        <>
>            <div className="overlay" onClick={onClose} />
>            <div className="modal-content">
>                {children}
>            </div>
>        </>,
>        portalElement
>    );
>};
>```
> - Порталы обычно используются для модальных окон, тултипов и всплывающих уведомлений
>