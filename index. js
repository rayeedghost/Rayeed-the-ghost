const { Client, GatewayIntentBits } = require('discord.js');
const { joinVoiceChannel, createAudioPlayer, createAudioResource } = require('@discordjs/voice');
const play = require('play-dl');

const client = new Client({
    intents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildVoiceStates,
        GatewayIntentBits.GuildMessages,
        GatewayIntentBits.MessageContent
    ]
});

client.once('ready', () => {
    console.log(`Music Bot is online as ${client.user.tag}!`);
});

client.on('messageCreate', async message => {
    if (message.author.bot) return;

    if (message.content.startsWith('!play')) {
        const args = message.content.split(' ');
        const url = args[1];

        if (!message.member.voice.channel) {
            return message.reply('ياهوو، ادخل روم صوتي أول!');
        }
        if (!url) {
            return message.reply('حط رابط الأغنية بعد الأمر!');
        }

        try {
            const channel = message.member.voice.channel;
            const connection = joinVoiceChannel({
                channelId: channel.id,
                guildId: channel.guild.id,
                adapterCreator: channel.guild.voiceAdapterCreator,
            });

            const stream = await play.stream(url);
            const resource = createAudioResource(stream.stream, { inputType: stream.type });
            const player = createAudioPlayer();

            player.play(resource);
            connection.subscribe(player);

            message.reply('ابشر، جالس أشغل المقطع...');
        } catch (error) {
            console.error(error);
            message.reply('صار خطأ وأنا أشغل الصوت.');
        }
    }
});

client.login(process.env.TOKEN);
