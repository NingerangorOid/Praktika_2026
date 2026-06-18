using System;

namespace Inheritance.MapObjects;

// 1. Интерфейсы компонентов взаимодействий
public interface IHasOwner
{
    int Owner { get; set; }
}

public interface IHasArmy
{
    Army Army { get; set; }
}

public interface IHasTreasure
{
    Treasure Treasure { get; set; }
}

// 2. Классы игровых объектов на карте
public class Dwelling : IHasOwner
{
    public int Owner { get; set; }
}

public class Mine : IHasOwner, IHasArmy, IHasTreasure
{
    public int Owner { get; set; }
    public Army Army { get; set; }
    public Treasure Treasure { get; set; }
}

public class Creeps : IHasArmy, IHasTreasure
{
    public Army Army { get; set; }
    public Treasure Treasure { get; set; }
}

public class Wolves : IHasArmy
{
    public Army Army { get; set; }
}

public class ResourcePile : IHasTreasure
{
    public Treasure Treasure { get; set; }
}

// 3. Сервис обработки игровых взаимодействий
public static class Interaction
{
    public static void Make(Player player, object mapObject)
    {
        if (player == null || mapObject == null) return;

        // Порядок важен: сначала проверяется армия (опасность гибели)
        if (mapObject is IHasArmy armyComponent && !player.CanBeat(armyComponent.Army))
        {
            player.Die();
            return;
        }

        if (mapObject is IHasOwner ownerComponent)
        {
            ownerComponent.Owner = player.Id;
        }

        if (mapObject is IHasTreasure treasureComponent)
        {
            player.Consume(treasureComponent.Treasure);
        }
    }
}
