package me.Melo.CustomItems;

import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.potion.PotionEffectType;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.entity.Entity;
import org.bukkit.Material;
import org.bukkit.NamespacedKey;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.enchantments.Enchantment;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.entity.EntityDamageByEntityEvent;
import org.bukkit.inventory.ItemFlag;
import org.bukkit.inventory.ItemStack;
import org.bukkit.inventory.ShapedRecipe;
import org.bukkit.inventory.meta.ItemMeta;

public class Main extends JavaPlugin implements Listener{
	
	Map<String,Long> cooldowns = new HashMap<String,Long>();
	
	@Override
	public void onEnable() {
		Bukkit.addRecipe(getMjolnirRecipe());
		Bukkit.broadcastMessage(ChatColor.DARK_PURPLE + "MelosCustomItems Ready");
		this.getServer().getPluginManager().registerEvents(this, this);
	}
	
	@Override
	public void onDisable() {
		
	}
	
	public boolean checkIfCrit(Player player) {
		if (player.isSprinting()) return false;
		if (player.isOnGround()) return false;
		if (player.hasPotionEffect(PotionEffectType.BLINDNESS))return false;
		if (!(player.getFallDistance() > 0)) return false;
		if (player.getLocation().getBlock().isLiquid()) return false;
		if (player.getVehicle() != null) return false;
		
		return true;
	}
	
	public ItemStack getMjolnirItemStack() {
		ItemStack item = new ItemStack(Material.NETHERITE_AXE);
		ItemMeta meta = item.getItemMeta();
		
		meta.addEnchant(Enchantment.DURABILITY, 1, true);
		meta.addItemFlags(ItemFlag.HIDE_ENCHANTS);
		
		meta.setDisplayName(ChatColor.YELLOW + "Mjolnir");
		
		ArrayList<String> lore = new ArrayList<String>();
		
		lore.add(ChatColor.GRAY + "Asgaurdian I");
		
		lore.add("\"You should know that when you");
		lore.add("betray me, I will kill you.\"");
		
		lore.add("\"Fortunately, I am mighty!\"");
		meta.setLore(lore);
		
		item.setItemMeta(meta);
		
		return item;
	}
	//Commands
	@Override
	public  boolean onCommand(CommandSender sender, Command cmd,String label,String[] args) {
		if (!(sender instanceof Player)) {
			sender.sendMessage("Console cannot run this command");
			return true;
		}
		Player player = (Player) sender;
		
		if (label.equalsIgnoreCase("getmjolnir")) {
			if (player.hasPermission("meloscustomitems.getmjolnir")) {
				
				if (!(player.getInventory().firstEmpty() != -1)) return false;
				player.sendMessage(ChatColor.YELLOW + "Item delivered!");
				player.getInventory().addItem(getMjolnirItemStack());
				return true;
			}
			else {
				player.sendMessage(ChatColor.RED + "No Permission");
				return true;
			}
		
		} 
		return false;
		
	}
	//EventHandler
	@EventHandler
	public void onPlayerAttack(EntityDamageByEntityEvent event) {
		Entity e = event.getEntity();
		Entity attacker = event.getDamager();
		//
		if(!(event.getDamager() instanceof Player))return;
		Player player = (Player) attacker;
		
		 if (!(checkIfCrit(player))) return; 
		
		//cooldown
		if (cooldowns.containsKey(player.getName())) {
			if (cooldowns.get(player.getName()) > System.currentTimeMillis()) return;
		}
		
		cooldowns.put(player.getName(), System.currentTimeMillis() + (10 * 1000));
		
		//
		ItemStack mainHand = player.getInventory().getItemInMainHand();
		if (mainHand == null) return;// check for item
		if (!mainHand.hasItemMeta()) return; //check for meta
		//
		ItemMeta meta = mainHand.getItemMeta();
		if (!meta.hasDisplayName()) return;//check for display name
		if (!meta.getDisplayName().contains("Mjolnir") || !meta.hasLore()) return;//check for name/lore
		//
		e.getWorld().strikeLightning(e.getLocation());
	}
	// recipes
	public ShapedRecipe getMjolnirRecipe() {
			
		ItemStack item = getMjolnirItemStack();
			
		NamespacedKey key = new NamespacedKey(this, "Mjolnir");
			
		ShapedRecipe recipe = new ShapedRecipe(key, item);
			
		recipe.shape(
				" N ",
				" A ",
				" G "
				);
		recipe.setIngredient('A',Material.NETHERITE_AXE);
		recipe.setIngredient('N',Material.NETHER_STAR);
		recipe.setIngredient('G', Material.ENCHANTED_GOLDEN_APPLE);
			
		return recipe;
	}
}
