Созданные изменения:

1. Реализуем удаление пользователей:

- В UserRepository(repository) переопределяем метод public boolean delete(Long id)

- В UserController(controller) прописываем метод public boolean deleteUser(Long userid)

- В UserView(view) добавляем командe DELETE (стр 44-47)

2. Перенес в UserController(controller). Сделал метод из private в public и добавил метод private String scan(String message) В

- В UserView внес изменения в строку 28 и 50: userController.createUser()

3. Слой dao логичнее всего перенести в repository

- Из интерфейса Operation необходимо все команды перенести в интерфейс GBRepository

- Далее перенести код из FileOperation в UserRepository

- После изменений код бужет выглядеть так:

  public class UserRepository implements GBRepository {
  private final UserMapper mapper;
  private final String fileName;
  public UserRepository(String fileName) {
  this.fileName = fileName;
  this.mapper = new UserMapper();
  try (FileWriter writer = new FileWriter(fileName, true)) {
  writer.flush();
  } catch (IOException e) {
  System.out.println(e.getMessage());
  }
  }

- Код из FileOperation в части методов readAll и saveAll перенес в UserRepository без изменений копированием

- В Main изменятся строки:
  FileOperation fileOperation = new FileOperation(DB_PATH);
  GBRepository repository = new UserRepository(fileOperation);

В измененном виде останется

GBRepository repository = new UserRepository(DB_PATH);

- во всем проекте удаляем import notebook.model.dao.impl.FileOperation;

- удаляем слой dao
