# Monitoria---Material-Back-End

## Criação da controller de login e cadastro 
using Microsoft.AspNetCore.Mvc;
using System.Linq;

public class UsuarioController : Controller
{
    private readonly AppDbContext _context;

    public UsuarioController(AppDbContext context)
    {
        _context = context;
    }

    // GET: Login (exibe a tela)
    [HttpGet]
    public IActionResult Login()
    {
        return View();
    }

    // POST: Login (valida usuário)
    [HttpPost]
    public IActionResult Login(string email, string senha)
    {
        var usuario = _context.Usuarios
            .FirstOrDefault(u => u.Email == email && u.SenhaHash == senha);

        if (usuario != null)
        {
            // Aqui você poderia salvar dados em sessão
            TempData["Mensagem"] = $"Bem-vindo {usuario.Nome}!";
            return RedirectToAction("Index", "Home");
        }
        else
        {
            ViewBag.Erro = "E-mail ou senha inválidos.";
            return View();
        }
    }

    // GET: Cadastro
    [HttpGet]
    public IActionResult Cadastro()
    {
        return View();
    }

    // POST: Cadastro
    [HttpPost]
    public IActionResult Cadastro(Usuario usuario)
    {
        if (ModelState.IsValid)
        {
            _context.Usuarios.Add(usuario);
            _context.SaveChanges();
            return RedirectToAction("Login");
        }

        return View(usuario);
    }
}
