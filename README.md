# Get-url-Joaquim
<?php
// Configuração do banco de dados
$host = 'localhost';
$dbname = 'nome_do_banco';
$username = 'usuario';
$password = 'senha';

// Conexão com o banco de dados
try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbname;charset=utf8", $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Erro na conexão: " . $e->getMessage());
}

// Verifica se os parâmetros estão na URL
if (isset($_GET['nome']) && isset($_GET['email'])) {
    $nome = $_GET['nome'];
    $email = $_GET['email'];

    // Validação básica (use métodos mais robustos em produção)
    if (!empty($nome) && filter_var($email, FILTER_VALIDATE_EMAIL)) {
        try {
            // Prepara e executa a inserção
            $stmt = $pdo->prepare("INSERT INTO usuarios (nome, email) VALUES (:nome, :email)");
            $stmt->bindParam(':nome', $nome);
            $stmt->bindParam(':email', $email);
            $stmt->execute();

            echo "Usuário inserido com sucesso!";
        } catch (PDOException $e) {
            echo "Erro ao inserir: " . $e->getMessage();
        }
    } else {
        echo "Dados inválidos.";
    }
} else {
    echo "Parâmetros ausentes.";
}
?>
