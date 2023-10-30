# view![image](https://github.com/fzkdiniz/view/assets/61026576/0cc5a19a-d825-4464-b02e-38cd4c3ec57c)



CREATE TABLE alunos (
    id_aluno INT PRIMARY KEY,
    Nome VARCHAR(150),
    email VARCHAR(150),
    curso INT, -- Chave estrangeira que se relaciona com id_curso na tabela "curso"
    FOREIGN KEY (curso) REFERENCES curso(id_curso)
);



CREATE TABLE curso (
    id_curso INT PRIMARY KEY,
    nome_curso VARCHAR(150),
    duracao_curso INT,
    mensalidade_curso INT
);


CREATE TABLE professores (
    id_professor INT PRIMARY KEY,
    nome VARCHAR(150),
    salario INT,
    curso_leciona INT, -- Chave estrangeira que se relaciona com id_curso na tabela "curso"
    FOREIGN KEY (curso_leciona) REFERENCES curso(id_curso)
);


CREATE OR REPLACE FUNCTION generate_student_email(new_name VARCHAR, new_id INT) RETURNS VARCHAR AS $$
DECLARE
    base_email VARCHAR;
BEGIN
    base_email := lower(new_name) || '.' || new_id || '@dominio.com';
    RETURN base_email;
END;
$$ LANGUAGE plpgsql;


CREATE OR REPLACE FUNCTION generate_student_email_trigger() RETURNS TRIGGER AS $$
BEGIN
    NEW.email := generate_student_email(NEW.Nome, NEW.id_aluno);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Criação do gatilho
CREATE TRIGGER on_insert_student
BEFORE INSERT ON alunos
FOR EACH ROW
EXECUTE FUNCTION generate_student_email_trigger();


CREATE OR REPLACE PROCEDURE inserir_curso(
    in id_curso INT,
    in nome_curso VARCHAR(150),
    in duracao_curso INT,
    in mensalidade_curso INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO curso (id_curso, nome_curso, duracao_curso, mensalidade_curso)
    VALUES (id_curso, nome_curso, duracao_curso, mensalidade_curso);
END;
$$;


CREATE OR REPLACE PROCEDURE inserir_curso(
    in id_curso INT,
    in nome_curso VARCHAR(150),
    in duracao_curso INT,
    in mensalidade_curso INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO curso (id_curso, nome_curso, duracao_curso, mensalidade_curso)
    VALUES (id_curso, nome_curso, duracao_curso, mensalidade_curso);
END;
$$;
