using System;
					
public class Program
{
	public static void Main()
	{
		Console.WriteLine("Hello World");
		
		ICanal whatsAppProfessor = new WhatsApp();
		ICanal instaAluno = new Instagram();
		
		
		MensagemBasica mensagem = new MensagemBasica();
		mensagem.DataEnvio = DateTime.Now;
		mensagem.Mensagem = "Olá";
		
		Video mensagemVideo =  new Video();
		mensagem.DataEnvio = DateTime.Now;
		mensagem.Mensagem = "Olá - Mensagem de vídeo";
		mensagemVideo.Arquivo = "fim de semana.mp4";
		mensagemVideo.Formato = TiposDeArquivo.MP3;
		mensagemVideo.Duracao = 60;
		
		//Enviando mensagem básica
		whatsAppProfessor.EnviarMensagem("+5511964687373", mensagem);
		
		//Enviando mensagem de vídeo
		instaAluno.EnviarMensagem("+5511964687373", mensagemVideo);
		
		
		MensagemMultimidia mensagemCaio = mensagemVideo;
		mensagemCaio.Mensagem = "Olá, Caio!";
		
		ICanal facebook = new Facebook();
		facebook.EnviarMensagem("usuarioDoCaio", mensagemCaio);
			
		
		
	}
}

public enum TiposDeArquivo
{
	MP3,
	MP4,
	PDF
}

public interface ICanal
{
	void EnviarMensagem(string destinatario, MensagemBasica mensagem);
	
	void EnviarMensagem(string destinatario, MensagemMultimidia mensagem);
	
	void EnviarMensagem(string destinatario, Video mensagem);
}

public abstract class CanaisMensagem : ICanal
{
	protected abstract string canal {get;}
	
	public void EnviarMensagem(string destinatario, MensagemBasica mensagem)
	{
	
	    Console.WriteLine("Mensagem básica enviada pelo canal: " + canal);
		Console.WriteLine("Destinatário: "+ destinatario);
		Console.WriteLine("Mensagem: "+ mensagem.Mensagem);
		Console.WriteLine("Data Envio: "+ mensagem.DataEnvio);
	}
	
	public void EnviarMensagem(string destinatario, MensagemMultimidia mensagem)
	{
	    Console.WriteLine("Mensagem multimidia enviada pelo canal: " + canal);
		Console.WriteLine("Destinatário: "+ destinatario);
		Console.WriteLine("Mensagem: "+ mensagem.Mensagem);
		Console.WriteLine("Data Envio: "+ mensagem.DataEnvio);
		Console.WriteLine("Arquivo: "+ mensagem.Arquivo);
		Console.WriteLine("Tipo Arquivo: "+ mensagem.Formato);		
	}
	
	public void EnviarMensagem(string destinatario, Video mensagem)
	{
	    Console.WriteLine("Mensagem video enviada pelo canal: " + canal);
		Console.WriteLine("Destinatário: "+ destinatario);
		Console.WriteLine("Mensagem: "+ mensagem.Mensagem);
		Console.WriteLine("Data Envio: "+ mensagem.DataEnvio);
		Console.WriteLine("Arquivo: "+ mensagem.Arquivo);
		Console.WriteLine("Tipo Arquivo: "+ mensagem.Formato);	
		Console.WriteLine("Duração: "+ mensagem.Duracao);	
	}
}

public class WhatsApp : CanaisMensagem, ICanal
{
	protected override string canal { get {return "Whats APP";}}
}

public class Telegram : CanaisMensagem, ICanal
{
	protected override string canal { get {return "Telegram";}}
}

public class Instagram : CanaisMensagem, ICanal
{
	protected override string canal { get {return "Instagram";}}
}

public class Facebook : CanaisMensagem, ICanal
{
		protected override string canal { get {return "Facebook";}}
}


public class MensagemBasica
{
	public string Mensagem {get; set;}
	public DateTime DataEnvio {get; set;}
}

public class MensagemMultimidia : MensagemBasica
{
	public string Arquivo {get; set;}
	public TiposDeArquivo Formato {get; set;}
}

public class Video : MensagemMultimidia
{
	public int Duracao {get; set;}
}
