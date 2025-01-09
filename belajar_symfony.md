# Belajar Symfony
Berikut adalah panduan untuk membuat proyek sederhana menggunakan Symfony: Aplikasi To-Do List. Proyek ini mencakup konsep dari silabus di atas, termasuk routing, controller, Doctrine ORM, form handling, dan templating dengan Twig.

## 1. Inisialisasi Proyek Symfony
Pastikan Symfony CLI telah terinstal.
Buat proyek baru:
```
symfony new todo_list --webapp
cd todo_list
```

## 2. Setup Database
Konfigurasikan database di file .env:
```
DATABASE_URL="mysql://user:password@127.0.0.1:3306/todo_list"
```

Jalankan perintah untuk membuat database:
```
php bin/console doctrine:database:create
```

## 3. Buat Entity Task
Entity ini akan merepresentasikan data to-do list di database.

Buat entity:

```
php bin/console make:entity Task
```

Tambahkan properti berikut:
- id: Integer (auto-increment).
- title: String.
- description: Text (opsional).
- isCompleted: Boolean (default false).

Hasilnya seperti ini:
```
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity]
class Task {
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private $id;

    #[ORM\Column(type: 'string', length: 255)]
    private $title;

    #[ORM\Column(type: 'text', nullable: true)]
    private $description;

    #[ORM\Column(type: 'boolean')]
    private $isCompleted = false;

    // Getters and setters...
}
```
Jalankan migrasi:

```
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

## 4. Buat Controller dan Routing
Buat controller untuk mengelola task:
```
php bin/console make:controller TaskController
```

Tambahkan metode berikut di TaskController:
- index(): Menampilkan daftar task.
- create(): Form untuk membuat task baru.
- toggle(): Menandai task sebagai selesai/tidak selesai.
- delete(): Menghapus task.
  
Contoh TaskController.php:
```
namespace App\Controller;

use App\Entity\Task;
use App\Form\TaskType;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class TaskController extends AbstractController {
    #[Route('/', name: 'task_index')]
    public function index(EntityManagerInterface $em): Response {
        $tasks = $em->getRepository(Task::class)->findAll();
        return $this->render('task/index.html.twig', ['tasks' => $tasks]);
    }

    #[Route('/create', name: 'task_create')]
    public function create(Request $request, EntityManagerInterface $em): Response {
        $task = new Task();
        $form = $this->createForm(TaskType::class, $task);
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            $em->persist($task);
            $em->flush();
            return $this->redirectToRoute('task_index');
        }

        return $this->render('task/create.html.twig', ['form' => $form->createView()]);
    }

    #[Route('/toggle/{id}', name: 'task_toggle')]
    public function toggle(Task $task, EntityManagerInterface $em): Response {
        $task->setIsCompleted(!$task->getIsCompleted());
        $em->flush();
        return $this->redirectToRoute('task_index');
    }

    #[Route('/delete/{id}', name: 'task_delete')]
    public function delete(Task $task, EntityManagerInterface $em): Response {
        $em->remove($task);
        $em->flush();
        return $this->redirectToRoute('task_index');
    }
}
```

## 5. Buat Form Task
Buat form untuk entitas Task:
```
php bin/console make:form TaskType
```
Sesuaikan file TaskType.php:
```
namespace App\Form;

use App\Entity\Task;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\Extension\Core\Type\CheckboxType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class TaskType extends AbstractType {
    public function buildForm(FormBuilderInterface $builder, array $options): void {
        $builder
            ->add('title', TextType::class)
            ->add('description', TextareaType::class, ['required' => false])
            ->add('isCompleted', CheckboxType::class, ['required' => false]);
    }

    public function configureOptions(OptionsResolver $resolver): void {
        $resolver->setDefaults(['data_class' => Task::class]);
    }
}
```
6. Tampilan dengan Twig
Buat file templates/task/index.html.twig:

```
{% extends 'base.html.twig' %}

{% block title %}To-Do List{% endblock %}

{% block body %}
    <h1>To-Do List</h1>
    <a href="{{ path('task_create') }}">Tambah Task</a>
    <ul>
        {% for task in tasks %}
            <li>
                <strong>{{ task.title }}</strong> - {{ task.description }}
                {% if task.isCompleted %} (Selesai) {% endif %}
                <a href="{{ path('task_toggle', { id: task.id }) }}">Toggle</a>
                <a href="{{ path('task_delete', { id: task.id }) }}">Hapus</a>
            </li>
        {% endfor %}
    </ul>
{% endblock %}
```

Buat file templates/task/create.html.twig:

```
{% extends 'base.html.twig' %}

{% block title %}Tambah Task{% endblock %}

{% block body %}
    <h1>Tambah Task Baru</h1>
    {{ form_start(form) }}
        {{ form_widget(form) }}
        <button type="submit">Simpan</button>
    {{ form_end(form) }}
{% endblock %}
```

## 7. Testing
Jalankan server:
```
symfony server:start
```

Akses aplikasi di browser: http://localhost:8000
Dengan langkah-langkah ini, Anda berhasil membuat aplikasi To-Do List sederhana dengan Symfony. Proyek ini mencakup dasar-dasar yang relevan untuk junior programmer. 🎉
