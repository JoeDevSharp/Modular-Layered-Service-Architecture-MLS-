# Proposition Modular Layered Service Architecture (MLS)
Dans le monde en constante évolution du développement logiciel, la conception d'une architecture adaptée aux besoins actuels est essentielle pour garantir la flexibilité, la modularité et la scalabilité des applications. Dans cette optique, nous proposons une approche novatrice : l'Architecture Modulaire de Services (MLS). Cette proposition vise à moderniser le développement d'applications en combinant les meilleures pratiques des architectures traditionnelles avec les exigences croissantes de l'industrie.

![AMS-Architecture AMS](https://github.com/JoeDevSharp/Modular-Layered-Service-Architecture-MLS-/assets/26557328/1ded2fe0-a85b-4bee-b935-b2d5c57622a0)

La MLS repose sur le principe fondamental de la modularité, permettant de décomposer les applications en services autonomes et réutilisables. En intégrant des couches bien définies pour la logique métier, l'accès aux données et la présentation, cette architecture offre une structure organisée et évolutive pour le développement logiciel. En outre, elle favorise l'intégration fluide avec des systèmes externes grâce à l'utilisation d'APIs bien conçues et la prise en charge de différents clients.

Dans cet article, nous explorerons en détail les principes, les composants et les avantages de la MLS, ainsi que des exemples pratiques de son application dans le développement d'applications modernes. En adoptant cette approche, les développeurs peuvent créer des applications plus flexibles, évolutives et faciles à maintenir, répondant ainsi aux défis complexes du paysage technologique actuelles.

Les différentes couches de cette architecture:

![AMS-Architecture AMS Layers](https://github.com/JoeDevSharp/Modular-Layered-Service-Architecture-MLS-/assets/26557328/5239f80a-e605-4f3d-9bb3-e0c12cb5887e)

L'objectif principal de cette architecture est de combiner la scalabilité horizontale des microservices et la polyvalence des applications monolithiques. Selon vos besoins en matière de consommation de votre application, vous adapterez la couche "Views" pour exposer ou non les API nécessaires.

Respectant le principe de "Single Responsibility", chacune des couches a une raison d'être, donc un objectif et une responsabilité unique. En séparant et en responsabilisant chaque couche dans son contexte, avec la règle d'or que chaque couche ne connaît que la couche supérieure.

## Repositories Layer

La raison d'être de cette couche est de moduler chaque repository avec ses entités et son contexte pour le rendre chaque action indépendant des autres. Ici un exemple de l'explorateur de solution en Visual Studio

![Captura de pantalla 2024-04-28 095032](https://github.com/JoeDevSharp/Modular-Layered-Service-Architecture-MLS-/assets/26557328/fe441196-089c-4960-a5dc-7fb438cf51c1)

Ici nous montrons la dissection des actions avec l'objective de leur responsabiliser qui implémenterait sont contrat correspondent. Ici un exemple du contrat de interface ICreated<T>:

````csharp
namespace ProductRepository.Contract
{
    public interface ICreated<Entity>
    {
        public Entity Add(Entity entity);
        public void AddRanger(ICollection entities);
    }
}
````

Ici un exemple de la clase ProductCreated qui implémente le contrat d'interface ICreated<T>:

````csharp
using ProductRepository.Contract;
using ProductRepository.Entities;

namespace ProductRepository : ICreated<Product>
{
    public class ProductCreated 
    {
        public Product Add(Product entity)
        {
            using (var context = new Context())
            {
                // ...
            }
        }

        public void AddRanger(ICollection entities)
        {
            using (var context = new Context())
            {
                // ...
            }
        }
    }
}
````

![AMS-Repository Layer](https://github.com/JoeDevSharp/Modular-Layered-Service-Architecture-MLS-/assets/26557328/6605801b-a9f6-4445-aeb6-198a815f3ed0)


## Services Layer

Dans l'architecture Modulaire de Services (MLS), la couche des Services joue un rôle crucial dans la logique métier de l'application. Cette couche encapsule les fonctionnalités principales de l'application et expose des opérations de service cohérentes et réutilisables.

![AMS-Layer Service and Repository](https://github.com/JoeDevSharp/Modular-Layered-Service-Architecture-MLS-/assets/26557328/f9366149-27cc-4f9f-b912-e0678b9b640d)


Chaque service est conçu pour remplir une fonction spécifique de l'application, telles que la gestion des utilisateurs, la manipulation des produits, ou d'autres aspects métier. Chaque service est autonome et indépendant des autres, favorisant ainsi la modularité et la réutilisation du code.

Ici un exemple d’implémentation du service ProductService, en utilisant chaque action dont nous avons besoin :

````csharp
using ProductRepository;
using ProductRepository.Entities;

namespace ProductService
{
    public class ProductService
    {
        private ProductCreated _productCreated;
        private ProductUpdate _productUpdate;

        public ProductService()
        {
            _productCreated = new ProductCreated();
            _productUpdate = new ProductUpdate();
        }

        public void Add()
        {
            _productCreated.Add(new Product()
            {
                Name = "Test",
            });
        }

        public void Update()
        {
            _productUpdate.Update(...);
        }
    }
}
````

## Views Layers

La couche View dans l'Architecture Modulaire de Services (MLS) est celle qui expose les API qui permettent l'interaction avec les différents services de l'application. Cette couche joue un rôle crucial dans la communication entre les clients externes et les services internes.

Les APIs exposées dans la couche View servent de point d'entrée pour les clients externes, tels que les applications Web, mobiles ou de bureau, ainsi que d'autres systèmes ou services externes. Ces APIs définissent les contrats et les points de terminaison permettant aux clients d'accéder aux fonctionnalités offertes par les services sous-jacents. Cette couche est utiliser que si le service doit etre exposer pour sa consumation par un client ou autre service.

![AMS-Architecture AMS](https://github.com/JoeDevSharp/Modular-Layered-Service-Architecture-MLS-/assets/26557328/625d173b-bd37-478d-97c0-321c79a64f22)


Comment dans cette exemple le Service 2 est utiliser par la API Gateway  qu'exposerait le services aux client et autres services externes, mais aussi par une application de console via référence du projet.

Client Layers

Les couches clientes dans l'Architecture Modulaire de Services (MLS) sont les interfaces utilisateur qui consomment les APIs exposées par la couche View ou directement le service via référence du projet si besoin. Ces couches sont responsables de fournir une expérience utilisateur intuitive et conviviale, tout en interagissant de manière transparente avec les fonctionnalités offertes par les services internes de l'application.

## Avantages :
- Modularité et Réutilisabilité : La MLS permet de décomposer l'application en services autonomes et réutilisables, favorisant ainsi la modularité du code et la réutilisation des fonctionnalités.
- Scalabilité : Les services indépendants peuvent être mis à l'échelle de manière séparée en fonction des besoins, ce qui permet une scalabilité plus granulaire et efficace.
- Flexibilité : En séparant les différentes couches de l'application, la MLS offre une grande flexibilité pour modifier, ajouter ou supprimer des fonctionnalités sans affecter l'ensemble du système.
- Interopérabilité : Les APIs bien définies permettent une intégration facile avec d'autres systèmes ou services externes, favorisant ainsi l'interopérabilité de l'application.
- Maintenabilité : La séparation des préoccupations et la clarté de la structure de l'application rendent la maintenance et le débogage plus faciles et efficaces.

## Inconvénients :
- Complexité initiale : La mise en place de l'architecture modulaire peut nécessiter un certain effort initial pour définir les différentes couches et les APIs entre elles.
- Surcoût : La gestion de plusieurs services autonomes peut entraîner des coûts supplémentaires en termes de gestion des déploiements, de surveillance et de maintenance.
- Coordination des services : Avec de nombreux services indépendants, la coordination et la gestion des dépendances entre eux peuvent devenir complexes et nécessiter une planification minutieuse.
- Performance : L'ajout de couches et de services peut entraîner une surcharge de performance, surtout si les appels entre les services sont fréquents et intensifs.
- Sécurité : La multiplicité des points d'accès et des APIs peut rendre la gestion de la sécurité plus complexe, nécessitant une attention particulière à la gestion des autorisations et des accès.

## Bonnes pratiques:

En résumé, bien que l'Architecture Modulaire de Services offre de nombreux avantages en termes de modularité, de scalabilité et de flexibilité, elle comporte également des défis en termes de complexité, de coûts et de gestion. Il est donc important d'évaluer attentivement les besoins et les contraintes de votre projet avant d'adopter cette approche architecturale.

- Cohésion des services : Concevez chaque service pour qu'il ait une seule responsabilité et qu'il fournisse une fonctionnalité spécifique de manière cohérente. Évitez les services monolithiques qui tentent de gérer plusieurs aspects du système.
- Découplage des services : Minimisez les dépendances entre les services en utilisant des interfaces claires et des contrats bien définis. Évitez les couplages forts qui rendent les services difficiles à modifier et à remplacer.
- Réutilisabilité du code : Favorisez la réutilisabilité du code en développant des services modulaires et indépendants. Identifiez les fonctionnalités communes qui peuvent être encapsulées dans des services réutilisables pour éviter la duplication du code.
- Isolation des erreurs : Concevez les services de manière à ce qu'ils soient isolés les uns des autres pour limiter l'impact des erreurs et des pannes. Utilisez des mécanismes de gestion des erreurs et de récupération pour assurer la résilience du système dans son ensemble.
- Testabilité : Concevez les services de manière à ce qu'ils soient faciles à tester de manière unitaire et à grande échelle. Adoptez des pratiques de test automatisé pour garantir la qualité et la fiabilité du code.
- Documentation : Documentez soigneusement chaque service, y compris ses fonctionnalités, ses contrats, ses dépendances et ses API. Fournissez des exemples d'utilisation et des guides de développement pour faciliter l'intégration et la compréhension par les développeurs.
- Surveillance et analyse : Mettez en place des mécanismes de surveillance et d'analyse pour suivre les performances, la disponibilité et l'utilisation des services. Utilisez ces informations pour optimiser les performances et identifier les points faibles du système.
- Évolutivité : Conceptionez les services pour qu'ils soient évolutifs et capables de gérer une charge de travail croissante. Utilisez des architectures distribuées et des technologies de mise à l'échelle horizontale pour garantir une croissance sans heurts du système.

En suivant ces bonnes pratiques, les équipes de développement peuvent créer des systèmes robustes, flexibles et maintenables basés sur l'Architecture Modulaire de Services. Cela permettra de garantir le succès à long terme du projet et de répondre aux besoins changeants des utilisateurs et des entreprises.

## Conclusion:

Dans un monde en constante évolution du développement logiciel, la conception d'une architecture adaptée aux besoins actuels est essentielle pour garantir la flexibilité, la modularité et la scalabilité des applications. L'Architecture Modulaire de Services représente une approche novatrice qui combine les meilleures pratiques des architectures traditionnelles avec les exigences croissantes de l'industrie.

En se basant sur le principe fondamental de la modularité, la MLS permet de décomposer les applications en services autonomes et réutilisables. Intégrant des couches bien définies pour la logique métier, l'accès aux données et la présentation, cette architecture offre une structure organisée et évolutive pour le développement logiciel. De plus, elle favorise l'intégration fluide avec des systèmes externes grâce à l'utilisation d'APIs bien conçues et la prise en charge de différents clients.
