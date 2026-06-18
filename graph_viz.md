```mermaid
classDiagram
    %% Главный класс-контейнер и перечисления
    class DotGraphBuilder {
        +DirectedGraph(string)\$ IGraphBuilder
        +UndirectedGraph(string)\$ IGraphBuilder
    }
    
    class NodeShape {
        <<enumeration>>
        Box
        Ellipse
    }

    %% Публичные Fluent API интерфейсы 
    class IGraphBuilder {
        <<interface>>
        +AddNode(string) INodeBuilder
        +AddEdge(string, string) IEdgeBuilder
        +Build() string
    }

    class INodeBuilder {
        <<interface>>
        +With(Action) IGraphBuilder
    }

    class IEdgeBuilder {
        <<interface>>
        +With(Action) IGraphBuilder
    }

    class INodeAttributesBuilder {
        <<interface>>
        +Color(string) INodeAttributesBuilder
        +FontSize(int) INodeAttributesBuilder
        +Label(string) INodeAttributesBuilder
        +Shape(NodeShape) INodeAttributesBuilder
    }

    class IEdgeAttributesBuilder {
        <<interface>>
        +Color(string) IEdgeAttributesBuilder
        +FontSize(int) IEdgeAttributesBuilder
        +Label(string) IEdgeAttributesBuilder
        +Weight(double) IEdgeAttributesBuilder
    }

    %% Вложенные классы
    class GraphBuilder {
        +Graph Graph
        +AddNode(string) INodeBuilder
        +AddEdge(string, string) IEdgeBuilder
        +Build() string
    }
    IGraphBuilder <|.. GraphBuilder : Реализует

    class NodeBuilder {
        -GraphBuilder graphBuilder
        -GraphNode node
        +With(Action) IGraphBuilder
    }
    INodeBuilder <|.. NodeBuilder : Реализует
    IGraphBuilder <|.. NodeBuilder : Позволяет продолжать цепь

    class EdgeBuilder {
        -GraphBuilder graphBuilder
        -GraphEdge edge
        +With(Action) IGraphBuilder
    }
    IEdgeBuilder <|.. EdgeBuilder : Реализует
    IGraphBuilder <|.. EdgeBuilder : Позволяет продолжать цепь

    class NodeAttributesBuilder {
        -Dictionary attributes
        +Color(string) INodeAttributesBuilder
        +FontSize(int) INodeAttributesBuilder
        +Label(string) INodeAttributesBuilder
        +Shape(NodeShape) INodeAttributesBuilder
    }
    INodeAttributesBuilder <|.. NodeAttributesBuilder : Реализует

    class EdgeAttributesBuilder {
        -Dictionary attributes
        +Color(string) IEdgeAttributesBuilder
        +FontSize(int) IEdgeAttributesBuilder
        +Label(string) IEdgeAttributesBuilder
        +Weight(double) IEdgeAttributesBuilder
    }
    IEdgeAttributesBuilder <|.. EdgeAttributesBuilder : Реализует

    %% Внешние доменные зависимости (модели самого Графа)
    class Graph
    class GraphNode {
        +Dictionary Attributes
    }
    class GraphEdge {
        +Dictionary Attributes
    }

    %% Вложенность классов внутри DotGraphBuilder
    DotGraphBuilder +-- GraphBuilder
    DotGraphBuilder +-- NodeBuilder
    DotGraphBuilder +-- EdgeBuilder
    DotGraphBuilder +-- NodeAttributesBuilder
    DotGraphBuilder +-- EdgeAttributesBuilder

    %% Перекрестные связи строителей (Переходы состояний)
    GraphBuilder --> Graph : Управляет структурой
    NodeBuilder --> GraphBuilder : Возвращает контекст
    NodeBuilder --> GraphNode : Конфигурирует
    EdgeBuilder --> GraphBuilder : Возвращает контекст
    EdgeBuilder --> GraphEdge : Конфигурирует
    NodeAttributesBuilder ..> NodeShape : Использует перечисление
```
