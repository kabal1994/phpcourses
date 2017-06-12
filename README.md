<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset= utf-8" />
    <title>Register</title>
</head>
    <body>
        <?php

    class AuthClass
    {
        private $_login = "Moran";
        private $_password = "q1w2e3r4t5";

        public function isAuth()
        {
            if (isset ($_SESSION["isAuth"])) { //если существет сессия
                return $_SESSION ["isAuth"]; // возврат перменной сессии (true - пользователь авторизован, false - не авторизован)
            } else return false; // пользователь не авторизован

        }

        public function auth($login, $password)
        {
            if ($login == $this->_login && $password == $this->_password) {
                $_SESSION ["isAuth"] = true; // делаем пользователя авторизированным
                $_SESSION ["login"] = $login; // записали в сесию логин пользовтеля
                return true;
            } else { // логин и пароль не верны
                $_SESSION["isAuth"] = false;
                return false;
            }
        }

            public function getLogin()
        {
            if ($this->isAuth()) {   // если пользователь авторизирован
                return $_SESSION{"login"}; //возвращаем логин записанный в сессию
            }
        }
           public function out()
        {
            $_SESSION = array(); //clear session
            session_destroy(); //destroy
        }

    }

        $auth  = new AuthClass();
    if (isset ($_POST{"login"})&& isset ($_POST["password"])){ //логин и пароль отправлены
        if (!$auth->auth($_POST["login"], $_POST["password"])){
            echo "<h2 style=\"Color:red;\"> Error</h2>";

        }
    }
    if (isset($_GET["is exit"])){
        if ($_GET["is exit"] == 1){
            $auth->out(); //exit
            header("Location: ?is_exit=0"); //Редирект после выхода
        }
    }
        ?>
    <?php

    if ($auth->isAuth()) { // Если пользователь авторизован, приветствуем:
        echo "Здравствуйте, " . $auth->getLogin() ;
        echo "<br/><br/><a href=\"?is_exit=1\">Выйти</a>"; //Показываем кнопку выхода
    }
else { //Если не авторизован, показываем форму ввода логина и пароля
?>

        <form method="post" action="">
            Логин:  <input type="text" name="login" value="<?php echo (isset($_POST["login"])) ? $_POST["login"] : null; // Заполняем поле по умолчанию ?>" /><br/><br/>
            Пароль: <input type="password" name="password" value="" /><br/>
            <input type="submit" value="Войти" />
        </form>
<?php } ?>


    </body>
</html>
