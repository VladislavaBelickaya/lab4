using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApplication1
{
    public class Token
    {
        public int Recipient { get; set; }
        public string Data { get; set; }
    }

    public class OThread
    {
        public int Id { get; set; }
        public virtual OThread Next { get; set; }
    }

    class Program
    {
        private const int ThreadLimit = 200;

        static void Main(string[] args)
        {
            var threads = new List<OThread> {};
            threads.Add(new OThread() { Id = 0, Next = null });
            for (int i = 1; i <= ThreadLimit-1; i++)
            {
                threads.Add(new OThread() { Id = i, Next = threads[i - 1] });
            }

            Thread thread = new Thread(() => { Thread(threads[ThreadLimit-1], new Token() { Recipient = 139, Data = "token" }); });
            thread.Start();
        }

        static void Thread(OThread oThread, Token t)
        {
            if (t.Recipient == oThread.Id)
            {
                Console.WriteLine(t.Data + " (" + oThread.Id + ")");
                Console.Read();
            }
            else
            {
                Thread thread = new Thread(() => Thread(oThread.Next, t));
                thread.Start();
            }
        }
    }
}
