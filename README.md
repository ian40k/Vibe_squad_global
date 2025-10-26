![Uploading 1752867646908.jpg…]()
# Vibe_squad_global
A commanded WhatsApp bot ,tyoes of command .ping , . ttsearch  and can be deployed on vercel
const { Client, LocalAuth } = require('whatsapp-web.js');
const qrcode = require('qrcode-terminal');
const axios = require('axios');

const client = new Client({
    authStrategy: new LocalAuth(),
    puppeteer: {
        headless: true,
        args: ['--no-sandbox', '--disable-setuid-sandbox']
    },
    webVersionCache: {
        type: 'remote',
        remotePath: 'https://raw.githubusercontent.com/wppconnect-team/wa-version/main/html/2.2412.54.html'
    }
});

// Device Linking QR Code
client.on('qr', (qr) => {
    console.log('📱 LINK YOUR DEVICE: Scan this QR with WhatsApp');
    console.log('📲 Go to WhatsApp → Linked Devices → Link a Device');
    qrcode.generate(qr, {small: true});
});

client.on('ready', () => {
    console.log('✅ Device Linked Successfully!');
    console.log('🤖 Bot is ready for commands!');
});

client.on('authenticated', () => {
    console.log('🔗 Device authentication successful!');
});

client.on('auth_failure', () => {
    console.log('❌ Device linking failed! Please try again.');
});

// All Commands
client.on('message', async (message) => {
    const text = message.body.toLowerCase();
    
    // .ping command
    if (text === '.ping') {
        message.reply('🏓 Pong! Bot is active and responsive!');
    }
    
    // .menu command
    else if (text === '.menu') {
        const menu = `
🤖 *BOT COMMANDS MENU*

🔄 *Basic Commands*
• .ping - Check bot status
• .menu - Show this menu
• .owner - Contact information

📥 *Download Commands*
• .ttsearch [query] - Search TikTok videos
• .ytsearch [query] - Search YouTube videos
• .igsearch [query] - Search Instagram content

🔧 *Utility Commands*
• .time - Current time
• .status - Bot system status

💡 Type any command to get started!
        `;
        message.reply(menu);
    }
    
    // .owner command
    else if (text === '.owner') {
        message.reply(`👑 *OWNER INFORMATION*
        
Name: Your Name
Contact: your@email.com
Support: Available 24/7`);
    }
    
    // .ttsearch command
    else if (text.startsWith('.ttsearch')) {
        const query = text.replace('.ttsearch', '').trim();
        if (!query) {
            message.reply('❌ Please provide a search query: .ttsearch [video name]');
        } else {
            message.reply(`🔍 Searching TikTok for: *${query}*\n⏳ Please wait...`);
            // Add TikTok search logic here
        }
    }
    
    // .time command
    else if (text === '.time') {
        const now = new Date();
        message.reply(`🕒 Current Time: ${now.toLocaleString()}`);
    }
    
    // .status command
    else if (text === '.status') {
        message.reply(`✅ *BOT STATUS*
        
🟢 Online and Active
📱 Device: Linked Successfully
⚡ Performance: Optimal
🛠️ Ready to serve commands!`);
    }
});

// Start the bot
console.log('🚀 Initializing WhatsApp Bot with Device Linking...');
client.initialize();
