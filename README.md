package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

import javax.persistence.*;
import java.util.List;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}

@Entity
class Item {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nome;

    // Getters e Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }
}

@Repository
interface ItemRepository extends JpaRepository<Item, Long> {}

@Service
class ItemService {
    @Autowired
    private ItemRepository repository;
    
    public Item salvar(Item item) {
        return repository.save(item);
    }

    public List<Item> listarTodos() {
        return repository.findAll();
    }
}

@RestController
@RequestMapping("/itens")
class ItemController {
    @Autowired
    private ItemService service;

    @PostMapping
    public Item adicionar(@RequestBody Item item) {
        return service.salvar(item);
    }

    @GetMapping
    public List<Item> listar() {
        return service.listarTodos();
    }
}
