# Hooks

## Crear un Hook
Para crear un hook, necesita:
- crear un archivo `custom/Espo/Custom/Hooks/{EntityName}/{HookName}.php`;
- declarar acción de tipo hook;
- vaciar el Caché en Adminitración.

## Tipos de Hook 

Los tipos de hook principales son:

- beforeSave;
- afterSave;
- beforeRemove;
- afterRemove;
- afterRelate;
- afterUnrelate;
- afterMassRelate.

### Nuevos Tipos de Hook
Puede usar su propio tipo de hook y accionarlo con

`$this->getEntityManager()->getHookManager()->process($entityType, $hookType, $entity, $options);`.

## Orden de Hook
Si tiene varios hooks, relacionados a un Tipo de Entidad y con el mismo tipo de hook, y el orden de ejecución es importante, puede configurar una propiedad `public static $order` en un valor entero.

Elevar orden - se ejecuta primero el hook con el número de orden más pequeño.

## Ejemplo
Este ejemplo configura el Nombre de Cuenta por nuevos Cables, si no es configurado. 

`custom/Espo/Custom/Hooks/Lead/AccountName.php`

```php
namespace Espo\Custom\Hooks\Lead;

use Espo\ORM\Entity;

class AccountName extends \Espo\Core\Hooks\Base
{    
    public function beforeSave(Entity $entity, array $options = array())
    {
        if ($entity->isNew() && !$entity->get('accountName')) { 
            $entity->set("accountName", "No Account");
        }
    }
}
```

## Hooks Globales
Si necesita aplicar un hook para todas las entidades, puede usar hooks comunes. Para esto, coloque su clase de hook en el directorio Común, por ejemplo: `custom/Espo/Custom/Hooks/Common/{HookName}.php`.
