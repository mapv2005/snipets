# Instrucciones para instalar y configurar la gema devise

## Paso 1: Agregar Devise al proyecto

```hash
bundle add devise
```

## Paso 2: Instalar la gema Devise

```hash
rails generate devise:install
```

## Paso 3: Generar el modelo para User

```hash
rails generate devise User
```

## Paso 4: Migrar la base de datos

```hash
rails db:migrate
```

## Paso 5: Crear vistas para poder personalizarlas (opcional)

```hash
rails generate devise:views
```

## Agregar navbar en un partial para devise y cargarlo en el layout

_Se debe crear el partial en la ruta app>assets>shared>_navbar.html.erb y agregar el siguiente código de Bootstrap_

```hash
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <%= link_to 'SisGem', root_path, class: 'navbar-brand' %>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
      </ul>
        <% if user_signed_in? %>
        <li class="nav-item">
          <%#= current_user.name %>
          Usuario: <%= current_user.email %>
        </li>
        <% end %>
          <% if user_signed_in? %>
            <li class="nav-item">
              <%= button_to 'Cerrar sesión', destroy_user_session_path, class: 'btn btn-outline-success', method: :delete %>
            </li>
          <% else %>
            <li class="nav-item">
              <%= link_to 'Iniciar sesión', new_user_session_path, class:"margen" %>
            </li>
            <li class="nav-item">
              <%= link_to 'Registro',  new_user_registration_path, class: 'btn btn-outline-success'%>
            </li>
          <% end %>
    </div>
  </div>
</nav>
```

## para que el navbar funcione, se debe agregar el CDN de bootstrap al header del layout

```hash
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
```

## Quitar las viñetas de los li

```hash
li {
  list-style: none;
}
```

## Agregar campos adicionales al modelo User

```hash
rails generate migration AddNameToUsers name:string
```

## Ejecutar la migración:

```hash
rails db:migrate
```

## Actualiza las vistas y controladores de Devise (opcional):


## crear el controlador de registration

```hash
rails generate controller Registrations
```

Editar las vistas de registro y edición (registrations/new.html.erb y registrations/edit.html.erb) para agregar un campo de entrada para el nombre.

En el controlador registrations_controller.rb, asegúrate de que el controlador esté configurado para permitir los parámetros name durante la creación y actualización de usuarios.

En este archivo, hereda de Devise::RegistrationsController y personaliza los métodos que desees modificar. Por ejemplo, puedes agregar o modificar los métodos new, create, edit, update, destroy, etc., según tus necesidades.

Aquí tienes un ejemplo de cómo podría lucir un registrations_controller.rb personalizado:


## Define métodos personalizados si es necesario

class RegistrationsController < Devise::RegistrationsController
 private
end

Asegúrate de que en tu archivo routes.rb esté configurado para utilizar el controlador personalizado de Devise. Debería verse algo como esto:

devise_for :users, controllers: { registrations: 'registrations' }

class RegistrationsController < Devise::RegistrationsController
  private

  def sign_up_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation)
  end

  def account_update_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation, :current_password)
  end
end

Actualiza las rutas de Devise:


devise_for :users, controllers: { registrations: 'registrations' }
Actualiza las vistas de Devise:

Finalmente, actualiza las vistas de Devise para mostrar el campo name donde sea necesario.
























